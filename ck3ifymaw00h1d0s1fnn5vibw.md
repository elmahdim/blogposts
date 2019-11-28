## Destructuring in JavaScript

**Destructuring assignment** is one of the most flexible, simplest feature in modern JavaScript. It's a JavaScript expression that makes it possible to unpack values from Arrays, properties from Objects, or even Function params, into distinct variables.

## Destructuring objects

We often reference to the same variable in several places in our codebase and we'd prefer to avoid declaring out the same variable over the over again. For instance, making requests to the server might require an authorization (token) in the header's request. Assume we have a `session` object the holds `token` and `refresh_token` keys for instance 

ES5:

```javascript
const token = session.token;
const refreshToken = session.refresh_token;
```

ES6:
```javascript
const {
  token,         // ↓ alias ↓
  refresh_token: refreshToken
} = session;
```

In the ES6 snippet above, the syntax becomes more clear. Just like you could declare multiple comma-separated variables with a single `let` statement, you can also declare multiple variables within curly braces of a destructuring expression. Also there are cases where we want to rename the property name into different one; in that case, you'll use a `: amAlias` . Above we used alias to change the property name from snake-case to camelCase.

## Function parameter

We can use destructuring in function parameters to unpack fields from objects passed as function parameter. If we wanted to implement a function that only takes into account certain properties of parameter, it might be a good idea to reference those properties explicitly by destructuring up front. Destructure everything up front, makes it easy to spot when input doesn't adhere to the contract of a function.

```javascript
const getFullName = ({ user }) => 
  user && `${user.firstName} ${user.lastName}`;

const state = {
    user: {
        firstName: 'Mahmoud',
        lastName: 'Elmahdi'   
    }
};

getFullName(state); // Mahmoud Elmahdi
```

## Parsing a returned value from a function

Whenever there’s a function that returns an object or an array, destructuring can make working with that returned value more concise to interact with. The following example shows a `getUser` function that returns an object with user's data, where we grab only the ones we're interested in: `isAdmin` and ignore the rest

```javascript
const getUser = () => ({
    id: 156,
    isAdmin: true,
    fullName: 'Mahmoud Elmahdi',
    email: 'abracadabra@email.com',
    languages: ['Arabic', 'English', 'Russian']
});

const { isAdmin } = getUser();
isAdmin && retrieveSensitiveData();
```

## Array destructuring

Destructuring arrays uses square brackets instead of curly braces. Instead of having to sprinkle your code with implementation details like `x = arr[0]`, with destructuring you can convey your meaning clearly and without explicitly referencing the indices, naming the values instead 

![Destructuring arrays](https://cdn.hashnode.com/res/hashnode/image/upload/v1574924989528/8o0c6XpWJ.png)

When destructuring arrays, we also can skip those properties we don't need to reference to 

![Destructuring arrays](https://cdn.hashnode.com/res/hashnode/image/upload/v1574925179610/_2A9IevNT.png)

## Useful tips

Add properties conditionally inside object literals with spread operator for properties: 

![Add properties conditionally.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1574925317578/PnX6Y7xqb.png)

Add elements inside array literals only if the element is _true_ (**not** `null`/`undefined` or empty) 


![Add elements inside array literals.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1574925351058/Of_qFMGEz.png)
