```metadata
{
    "title": "Cypressでhoverを実現する方法",
    "date": "2022-01-17T15:24:07",
    "categories": "Cypress, React, TypeScript",
}
```

こちらで実現できました

```typescript
 // selectorには任意の値入れてください
cy.get('#removeButton').trigger('mouseover').click({force: true});
```

## 参考記事

[Trigger](https://docs.cypress.io/api/commands/hover#Trigger)
