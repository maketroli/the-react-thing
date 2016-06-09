# the-react-thing

## React:

### *NO es un MVC Framework. React es una librería para construir componentes de interfaces reusables y componentizadas que se actualizan a medida que sus datos subyacentes cambian en el tiempo.*

**Entidades fuertes usando Reactjs:**

Facebook (donde se creó), Netflix, PayPal, AirBnB, Coursera, Asana, Atlassian, Dropbox, Expedia, Flipboard, HipChat, Instagram, Khan Academy, Periscope, Reddit, Salesforce, Twitter - Fabric, Uber, Venmo, Whatsapp, Yahoo, Zendesk y algunitos mas.

### Especificaciones de Componentes.

Los principales son:

**render()**: La función render debe retornar un solo objeto en representacion de la interfaz. Tambien se puede retornar `null` o `undefined` para no mostrar ninguna interfaz.

**constructor()**: Se ejecuta una sola vez al inicio de montarse el componente en la pantalla y debe retornar el estado inicial del componente mendiante un objeto javascript.

**propTypes**: es un objeto que indica los tipos de las propiedades que recibe el componente en su instanciacion.

### Funciones del Ciclo de Vida

Estas funciones son ejecutadas en varios momentos del ciclo de vida del componente.

**componentDidMount()**: Se ejecuta una sola vez cuando el componente se monta en la página.

**componentDidUpdate()**: Se ejecuta cada vez que el componente se refresca y se ejecuta la función render

**Otras**: `componentWillMount`, `componentWillReceiveProps`, `shouldComponentUpdate`, `componentWillUpdate`, `componentWillUnmount`.

### Funciones de Operación

Estas funciones permiten cambiar el estado del componente o forzar que se refresque.

**setState()**: Permite cambiar el estado actual del componente, forzando con el mismo que se intente realizar un repintado del mismo de acuerdo al estado que se cambie. Adicionalmente se le puede especificar una función de callback para una vez termine el cambio de estado poder realizar operaciones.

**forceUpdate (No debe ser usada)**: Fuerza el repintado del componente manteniendo el estado anterior.

**Otras**: `replaceState`, `getDOMNode`, `isMounted`, `setProps`, `replaceProps`

Las funciones y metodos que he descrito son las que más he usado, normalmente son las esenciales para cualquier componente.

Un componente luce más o menos así:

```javascript
class Button extends React.Component {
  constructor () {
    super();
    this.state = { amount: 0 };
  }

  render () {
    return (
      <button type="button" onClick={this.adding}>
        Pressed {this.state.amount} times
      </button>);
  }
  
  adding = (evt) => {
    this.setState({amount: this.state.amount + 1});
    evt.preventDefault();
  }
};

React.render(<Button/>, document.getElementById('container'));
```

## Ejemplos de Código

Este ejemplo es usado para mostrar un simple componente que es un botón.
Acá se verán las maneras para esconder y mostrar un elemento. Y como cambiar el `state` de nuestro componente.

```javascript
/* jshint esnext: true */

class Button extends React.Component {

  constructor() {
    super();
    this.state = {amount: 0}; // Se setea el estado inicial del componente
  }

  render () {

    let showEl;
    let showEl2;
    let showEl3;

    // Los elementos se mostrarán si el state.amount es mayor a 5
    showEl = (this.state.amount > 5) ? <p>Amount 1: {this.state.amount}</p> : null;
    // showEl2 no tiene ningún condicional
    // Su condicional para que aparezca está dentro del return
    showEl2 = <p>Amount 2: {this.state.amount + 5}</p>

    if (this.state.amount > 5) {
      showEl3 = <button type="button" className="btn btn-default" onClick={this.addition}>
        New Button
      </button>
    }

    return (
        <div className="jumbotron container">

          {this.state.amount < 6
            <button type="button" className="btn btn-default" onClick={this.addition}>
              Pressed {this.state.amount} times
            </button>
          }
          // Acá se llaman los elementos que estaban escondidos
          // Aparecerán hasta que el state.amount cumpla con el condicional
          {showEl}
          {(this.state.amount > 5) && showEl2}
          {showEl3}
          
       </div>
    );
  }

  addition = (evt) => {
    this.setState({amount: this.state.amount + 1});
    evt.preventDefault();
  }
};

React.render(<Button/>, document.getElementById('container'));
```

# App `todo` List completa para ver como se mueven los `props` de un componente a otro

```javascript
/* jshint esnext: true */

class TodoList extends React.Component {

  static propTypes = {
    todos: React.PropTypes.array
  }

  constructor(props) {
    super(props)
    this.state = { todos: this.props.todos || [] }
  }
  
  addTodo = (item) => {
    this.setState({todos: this.state.todos.concat([item])});
  }

  render () {
    return (
      <div>
        <h3>TODO List</h3>
        <TodoItems items={this.state.todos}/>
        <TodoInput addTodo={this.addTodo}/>
      </div>
    );    
  }
};

class TodoItems extends React.Component {

  static propTypes = {
    items: React.PropTypes.array.isRequired
  }
  
  constructor(props) {
    super(props);
  }
  
  render () {
	let createItem;
      
    createItem = (item, index) => {
      return (
        <li key={index}>{item}</li>
      );
    };
	return <ul>{this.props.items.map(createItem)}</ul>;
  }
};

class TodoInput extends React.Component {
  
  constructor (props) {
     super(props);
     this.state = {item: ''};
  }
  
  onChange = (e) => {
    this.setState({item: e.target.value});
  }
  
  handleSubmit = (e) => {
    e.preventDefault();
    this.props.addTodo(this.state.item);
    this.setState({item: ''}, function() {
      React.findDOMNode(this.refs.item).focus();
    });
  }
  
  render () {
    return (
      <form onSubmit={this.handleSubmit}>
        <input type="text" onChange={this.onChange} value={this.state.item}/>
        <input type="submit" value="Add"/>
      </form>
    );
  }
};

   
React.render(<TodoList todos={['red','blue']}/>, document.getElementById('container'));
```
