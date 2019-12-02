## React add-on components

React is one if not the most popular JavaScript framework out there, however it doesn’t come with helpers out of the box like other frameworks does (i.e. *directives* in Vue.js).

I’ll share with you in this post **4 useful and reusable react components** to maximize your coding efficiency.


### Conditional render

The `If` component, a functional component with a `condition` and `otherwise` properties. 

The `condition` property is a pretty straightforward conditional statement. If the given condition is truthy a predefined [children property](https://reactjs.org/docs/glossary.html#propschildren) will be returned or else whatever values passed into the `otherwise` property will be rendered or nothing at all

#### Usage

```js
<If condition={flag} otherwise="render that">
  render this...
</If>
```

#### If.js

```js
import React from 'react';
import Proptypes from 'prop-types';

function If(props) {
  return props.condition ? props.children : props.otherwise;
}

If.propsTypes = {
  condition: Proptypes.bool,
  otherwise: Proptypes.oneOfType([
      Proptypes.arrayOf(Proptypes.node),
      Proptypes.node
  ]),
  children: Proptypes.oneOfType([
    Proptypes.arrayOf(Proptypes.node),
    Proptypes.node
  ])
};

If.defaultProps = {
  otherwise: null
};

export default If;
```

> I'm assuming you already have `prop-types` or whatever type checks package installed in your application.

### Broken images

The `Image` component replaces the broken `src` of an image with a `fallback` property (image) as its default placeholder.

#### Usage

```js
<Image src={pathToBrokentImage} alt="..." />
```

#### Image.js

```js
import React from 'react';
import Proptypes from 'prop-types';

function Image(props) {
  const { fallback, alt, ...rest } = props;
  const handleBrokenImage = (event) => event.target.src = fallback;

  return <img {...rest} alt={alt} onError={handleBrokenImage} />;
}

Image.propTypes = {
  fallback: Proptypes.string,
  alt: Proptypes.string,
};

Image.defaultProps = {
  alt: 'Default alt for a11y',
  fallback: 'path/to/default/image/or/placeholder'
};

export default Image;
```

##### Util

I'll create a simple arrow function as a util to use in the next two components, to generate a random [key](https://reactjs.org/docs/lists-and-keys.html#keys) for each element since we're going to iterate over a list of data to elements (*to prevent any warn/error logs in the console* )

```js
const getRandomKey = () => Math.random().toString(36).substr(2, 3);
```

### Mapping an array to elements

The `For` component iterates over the `of` property that accepts an array of data, this could be list of strings or list objects.

#### Usage

```js
const data = ['...', '...', '...'];

<For of={data} type='p' />

const anotherData = [
  {
   label: '...',
   value: '...',
  }
  {
   label: '...',
   value: '...',
  }
  {
   label: '...',
   value: '...',
  }
];

<For of={anotherData} type='li' parent='ul' iteratee='label' />
```

If no `iteratee` property provided! the component will return the first key value of each object within the array.

#### For.js

```js
import React, { PureComponent, createElement } from 'react';
import Proptypes from 'prop-types';

export default class For extends PureComponent {
  static propTypes = {
    of: Proptypes.array,
    type: Proptypes.string.isRequired,
    parent: Proptypes.string,
    iteratee: Proptypes.string,
  };

  getIteratee = (item) => {
    return item[this.props.iteratee] || item[Object.keys(item)[0]];
  };

  list = () => {
    const { of, type } = this.props;
    return of.map((item) => {
      const children = typeof item === 'object' ? this.getIteratee(item) : item;
      return createElement(type, {
        key: getRandomKey()
      }, children)
    })
  };

  children = () => {
    const { parent } = this.props;
    return parent ? createElement(parent, null, this.list()) : this.list();
  };

  render() {
    return this.props.of.length ? this.children() : null;
  }
}
```

### Data table

A basic `Table` component that renders data table with `headers`, `body` and `footer`.

#### Usage

```js
const data = {
  headers: ['...', '...'],
  body: [
    ['...', '...'],
    ['...', '...'],  
  ],
  footer: ['...', '...'],
};

<Table {...data} />
```

#### Table.js

*you can make it more challengeable by adding more options, for example a variety of table layout and more...*

```js
import React from 'react';
import Proptypes from 'prop-types';

export default class Table extends React.PureComponent {
  static propTypes = {
    header: Proptypes.array,
    body: Proptypes.array,
    footer: Proptypes.array,
  };

  static defaultProps = {
    header: [],
    body: [],
    footer: [],
  };

  static Cells = ({ data = [], cell = 'td' }) => data.length ? (
      data.map((d) => (
          cell === 'th' ?
              <th key={`th-${getRandomKey()}`}>{d}</th> :
              <td key={`td-${getRandomKey()}`}>{d}</td>
      ))
  ) : null;

  render() {
    const { header, body, footer, ...rest } = this.props;
    const bodyRows = body.map((row) => (
        <tr key={`th-${getRandomKey()}`}>
          <Table.Cells data={row} />
        </tr>
    ));

    return (
        <table {...rest}>
          {header.length ? (
              <thead>
                <tr>
                  <Table.Cells data={header} cell="th" />
                </tr>
              </thead>
          ) : null}
          {body.length ? <tbody>{bodyRows}</tbody> : null}
          {footer.length ? (
              <tfoot>
                <tr>
                  <Table.Cells data={footer} />
                </tr>
              </tfoot>
          ) : null}
        </table>
    )
  }
}
```

### Demo

I’ve made a simple application to play with. It has a several sections as you can tell from the demo below. Each component has sample test, feel free to fork and play around with the code. 

%[https://codesandbox.io/embed/react-add-on-components-g0flc?fontsize=14&hidenavigation=1&theme=dark]

---

Feedback are welcome. If you have any suggestions or corrections to make, please do not hesitate to drop me a note/comment.
