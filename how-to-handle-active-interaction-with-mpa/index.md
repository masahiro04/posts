```metadata
{
    "title": "Active InteractionをMPAでハンドリングする方法",
    "date": "2022-06-26T15:34:00",
    "categories": "Ruby on Rails",
}
```

 普段はAPI開発で利用しているので、Active recordのerrorsに入れるというよりもMapやList形式でエラーを返しているのですが、createが失敗した時のnewをレンダリングする際にちょこっとはまったので記事にしました

まずは最終的なコード

```ruby
 # interaction
module Resources
  module InvoiceInformations
    class Create < ActiveInteraction::Base
      hash :params do
        string :name
        string :zip
        string :prefecture
        string :city
        string :street
        string :building, default: nil
        string :memo, default: nil
      end

      object :current_user, class: User
      validates :current_user, presence: true

      def execute
        invoice_information = current_user.build_invoice_information(params)
        invoice_information.save!
      rescue ActiveRecord::RecordInvalid => e
        # ここ
        e.record.errors.errors.each do |error|
          errors.add(error.attribute, error.message)
        end
        # 公式では下記になってる
        # errors.merge!(e.record.errors.errors)
      end
    end
  end
end

# controller
def create
    outcome = Resources::InvoiceInformations::Create.run(
      params: params_create,
      current_user: current_user
    )
    if outcome.valid?
      @invoice_information = outcome.result
      redirect_to customers_path,
                  notice: t('action_messages.created', model_name: InvoiceInformation.model_name.human)
    else
      @invoice_information = current_user.build_invoice_information(outcome.params)
      # 正直もっと良い方法ありそうだけど、時間の関係上こちらで
      outcome.errors.errors.each do |e|
        @invoice_information.errors.add(e.attribute, e.message)
      end
      render :new
    end
  end
```

## 問題点

attributeにbaseが入れられてしまう、が問題です

```
 errors.merge!(e.record.errors.errors) でも当然エラーを返すことができるのですが、errors.merge!に公式の通りのコードを入れると、エラーの対象が特定のカラムではなく全てbaseで表現されてしまいます
```

煩雑になってしまいますが、丁寧に一つずつエラーをaddすることで目的の挙動を実現できます

※rescue内部でデバッグ実行してclassを確認

<strong>公式のコード</strong>

```ruby
 <ActiveModel::Error attribute=base, type=blank, options={}>
```

<strong>上記のコードの場合</strong>

```ruby
 <ActiveModel::Error attribute=zip, type=blank, options={}>
```

## まとめ

Active Interactionを利用することでusecase的な実装を行うことができるようになり、コードは増えますがより変更に耐えうる実装ができたかと思います

正直Railsを利用するのは否定的ですが、usecaseとして切り出せるのであればまだまだ可能性を感じるので、もうちょい研究とかしていきたいです


