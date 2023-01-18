# CODING GUIDE

The following document provides the coding guidelines for the different stacks of technologies that we are using to build our applications. The idea is to have guidelines, tools, and frameworks that help to create a clean code easy to maintain.

You are very welcome to add new ideas that you consider can improve our code delivery and also have tools for coding review.

## Table of Contents

1. [Typescript](#Typescript)
1. [React](#React)
1. [CSS&SASS](#CSSSASS)
1. [Clean Code](#Clean-Code)

## Typescript

There are some Typescript guide took it on the officially documentation, with some recommendation about to our coding experiences.

## Operators (TypeScript-specific and draft JavaScript)

- ?? (nullish coalescing)

```typescript
function getValue(val?: number): number | 'nil' {
  // Will return 'nil' if `val` is falsy (including 0)
  // return val || 'nil';

  // Will only return 'nil' if `val` is null or undefined
  return val ?? 'nil'
}
```

- ?. (optional chaining)

```typescript
function getValue(val?: number): number | 'nil' {
  // Will return 'nil' if `val` is falsy (including 0)
  // return val || 'nil';

  // Will only return 'nil' if `val` is null or undefined
  return val ?? 'nil'
}
```

- ! (null assertion)

```typescript
let value: string | undefined

// ... Code that we're sure will initialize `value` ...

// Assert that `value` is defined
console.log(`value is ${value!.length} characters long`)
```

## Basic Types

| Type                                                       | Typescript notation |
| ---------------------------------------------------------- | :-----------------: |
| Untyped                                                    |         any         |
| String                                                     |       string        |
| Number                                                     |       number        |
| Template string                                            |   ${string}Suffix   |
| true / false value                                         |       boolean       |
| non-primitive value                                        |       object        |
| Uninitialized value                                        |      undefined      |
| Explicitly empty value                                     |        null         |
| Null or undefined (usually only used for function returns) |        void         |
| Value that can never occur                                 |        Never        |
| Value with an unknown type                                 |       unknown       |

## Object Types

- Object

```typescript
{
    requiredStringVal: string;
    optionalNum?: number;
    readonly readOnlyBool: bool;
}
```

- Object with arbitrary string properties (like a hashmap or dictionary)

```typescript
{ [key: string]: Type; }
{ [key: number]: Type; }
{ [key: symbol]: Type; }
{ [key: `data-${string}`]: Type; }
```

## Literal types

- String

```typescript
let direction: 'left' | 'right'
```

## Numeric

```typescript
let roll: 1 | 2 | 3 | 4 | 5 | 6
```

## Arrays and Tuples

- Array of strings

```typescript
string[]
```

or

```typescript
Array<string>
```

- Array of functions that return strings

```typescript
(() => string)[]
```

```typescript
{ (): string; }[]
```

```typescript
Array<() => string>
```

- Basic tuples

```typescript
let myTuple: [string, number, boolean?]
```

```typescript
myTuple = ['test', 42]
```

- Variadic tuples

```typescript
type Numbers = [number, number]
type Strings = [string, string]

type NumbersAndStrings = [...Numbers, ...Strings]
// [number, number, string, string]

type NumberAndRest = [number, ...string[]]
// [number, varying number of string]

type RestAndBoolean = [...any[], boolean]
// [varying number of any, boolean]
```

- Named tuples

```typescript
type Vector2D = [x: number, y: number]
function createVector2d(...args: Vector2D) {}
// function createVector2d(x: number, y: number): void
```

## Functions

- Function type

```typescript
;(arg1: Type, argN: Type) => Type
```

```typescript
{ (arg1: Type, argN: Type): Type; }
```

- Constructor

```typescript
new () => ConstructedType;
```

```typescript
{ new (): ConstructedType; }
```

- Function type with optional param

```typescript
;(arg1: Type, optional?: Type) => ReturnType
```

- Function type with rest param

```typescript
;(arg1: Type, ...allOtherArgs: Type[]) => ReturnType
```

- Function type with static property

```typescript
{ (): Type; staticProp: Type; }
```

- Default argument

```typescript
function fn(arg1 = 'default'): ReturnType {}
```

- Arrow function

```typescript
;(arg1: Type): ReturnType => value
```

- this typing

```typescript
function fn(this: Foo, arg1: string) {}
```

- Overloads

```typescript
function conv(a: string): number;
function conv(a: number): string;
function conv(a: string | number): string | number {
    ...
}
```

## Union and intersection types

- Union

```typescript
let myUnionVariable: number | string
```

- Intersection

```typescript
let myIntersectionType: Foo & Bar
```

## Named types

- Interface

```typescript
interface Child extends Parent, SomeClass {
  property: Type
  optionalProp?: Type
  optionalMethod?(arg1: Type): ReturnType
}
```

- Class

```typescript
class Child extends Parent implements Child, OtherChild {
  property: Type
  defaultProperty = 'default value'
  private _privateProperty: Type
  private readonly _privateReadonlyProperty: Type
  static staticProperty: Type

  static {
    try {
      Child.staticProperty = calcStaticProp()
    } catch {
      Child.staticProperty = defaultValue
    }
  }

  constructor(arg1: Type) {
    super(arg1)
  }

  private _privateMethod(): Type {}

  methodProperty: (arg1: Type) => ReturnType
  overloadedMethod(arg1: Type): ReturnType
  overloadedMethod(arg1: OtherType): ReturnType
  overloadedMethod(arg1: CommonT): CommonReturnT {}
  static staticMethod(): ReturnType {}
  subclassedMethod(arg1: Type): ReturnType {
    super.subclassedMethod(arg1)
  }
}
```

- Enum

```typescript
enum Options {
  FIRST,
  EXPLICIT = 1,
  BOOLEAN = Options.FIRST | Options.EXPLICIT,
  COMPUTED = getValue()
}

enum Colors {
  Red = '#FF0000',
  Green = '#00FF00',
  Blue = '#0000FF'
}
```

- Type alias

```typescript
type Name = string

type Direction = 'left' | 'right'

type ElementCreator = (type: string) => Element

type Point = { x: number; y: number }

type Point3D = Point & { z: number }

type PointProp = keyof Point // 'x' | 'y'

const point: Point = { x: 1, y: 2 }

type PtValProp = keyof typeof point // 'x' | 'y'
```

## Generics

- Function using type parameters

```typescript
<T>(items: T[], callback: (item: T) => T): T[]
```

- Interface with multiple types

```typescript
interface Pair<T1, T2> {
  first: T1
  second: T2
}
```

- Constrained type parameter

```typescript
<T extends ConstrainedType>(): T
```

- Default type parameter

```typescript
<T = DefaultType>(): T
```

- Constrained and default type parameter

```typescript
<T extends ConstrainedType = DefaultType>(): T
```

- Generic tuples

```typescript
type Arr = readonly any[]

function concat<U extends Arr, V extends Arr>(a: U, b: V): [...U, ...V] {
  return [...a, ...b]
}

const strictResult = concat([1, 2] as const, ['3', '4'] as const)
const relaxedResult = concat([1, 2], ['3', '4'])

// strictResult is of type [1, 2, '3', '4']
```

## Utility types

- Partial

```typescript
Partial<{ x: number; y: number; z: number }>
```

is equivalent to

```typescript
{ x?: number; y?: number; z?: number; }
```

- Readonly

```typescript
Readonly<{ x: number; y: number; z: number }>
```

```typescript
{
    readonly x: number;
    readonly y: number;
    readonly z: number;
}
```

- Pick

```typescript
Pick<{ x: number; y: number; z: number }, 'x' | 'y'>
```

Is equivalent to

```typescript
{
  x: number
  y: number
}
```

## Type guards

- Type predicates

```typescript
function isThing(val: unknown): val is Thing {
  // return true if val is a Thing
}

if (isThing(value)) {
  // value is of type Thing
}
```

- typeof

```typescript

declare value: string | number | boolean;
const isBoolean = typeof value === “boolean”;

if (typeof value === "number") {
    // value is of type Number
} else if (isBoolean) {
    // value is of type Boolean
} else {
    // value is a string
}
```

- instanceof

```typescript
declare
value: Date | Error | MyClass

const isMyClass = value instanceof MyClass

if (value instanceof Date) {
  // value is a Date
} else if (isMyClass) {
  // value is an instance of MyClass
} else {
  // value is an Error
}
```

- in

```typescript
interface Dog {
  woof(): void
}
interface Cat {
  meow(): void
}

function speak(pet: Dog | Cat) {
  if ('woof' in pet) {
    pet.woof()
  } else {
    pet.meow()
  }
}
```

## Casting

- Type

```typescript
let val = someValue as string
```

or

```typescript
let val = <string>someValue
```

- Const (immutable value)

```typescript
let point = { x: 20, y: 30 } as const
```

or

```typescript
let point = <const>{ x: 20, y: 30 }
```

## React

- Naming Conventions

```jsx
Header.tsx
HeroBanner.tsx
CookieBanner.tsx
BlogListing.tsx
```

- Non-components should be written using camel case

```jsx
src / utils / form.ts
src / hooks / useForm.ts
src / components / banners / edit / Form.tsx
```

- Unit test files should use the same name as its corresponding file

```jsx
CookieBanner.tsx
CookieBanner.test.tsx

fetchData.tsx
fetchData.test.tsx
```

- Attribute name should be camel case

```jsx
className
onClick
```

- Variable names should be camel case

```jsx
const variable = 'test'
let variableBoolean = true
```

- CSS files should be named the same as the component

```jsx
CookieBanner.css
Header.css
```

- If a component requires multiple files (css, test) locate all files within component a folder

- Use .jsx or .tsx extension a for React components

- Use linting and automatic formatter
  Preferred tools are eslint and prettier.

- Import order
  Add some import order rules to your eslint config. This will ensure that the imports are always in the same order and are grouped by predefined rules.

```js
{
  'import/order': [
    2,
    {
      'newlines-between': 'always',
      groups: [
        'builtin',
        'external',
        'internal',
        'parent',
        'sibling',
        'index',
        'unknown',
        'object',
        'type',
      ],
      alphabetize: {
        order: 'asc',
        caseInsensitive: true,
      },
      pathGroups: [
        {
          pattern: 'react*',
          group: 'external',
          position: 'before',
        },
      ],
    },
  ],
}
```

- Naming conventions

Use PascalCase in components, interfaces, or type aliases

```jsx
// React component
const BannersEditForm = () => {
  ...
}

// Typescript interface
interface TodoItem {
  id: number;
  name: string;
  value: string;
}

// Typescript type alias
type TodoList = TodoItem[];
```

- Use camelCase for JavaScript data types like variables, arrays, objects, functions, etc.

```jsx
const getLastDigit = () => { ... }

const userTypes = [ ... ]
```

- Prefer using template literals

```jsx
// Not recommend
const userName = user.firstName + ' ' + user.lastName

// Recommend
const userDetails = `${user.firstName} ${user.lastName}`
```

- Use implicit return in small functions

```jsx
// Bad
const add = (a, b) => {
  return a + b
}

// Good
const add = (a, b) => a + b
```

- Avoid default export

```jsx
// bad
export default MyComponent;

// good
export { MyComponent };
export const MyComponent = ...;
export type MyComponentType = ...;
```

- Component structure

```jsx
// 1. Imports - Prefer destructuring imports to minimize writen code
import React, { PropsWithChildren, useState, useEffect } from 'react'

// 2. Types
type ComponentProps = {
  someProperty: string
}

// 3. Styles - with @mui use styled API or sx prop of the component
const Wrapper = styled('div')(({ theme }) => ({
  color: theme.palette.white
}))

// 4. Additional variables
const SOME_CONSTANT = 'something'

// 5. Component
function Component({ someProperty }: PropsWithChildren<ComponentProps>) {
  // 5.1 Definitions
  const [state, setState] = useState(true)
  const { something } = useSomething()

  // 5.2 Functions
  function handleToggleState() {
    setState(!state)
  }

  // 5.3 Effects
  // bad
  React.useEffect(() => {
    // ...
  }, [])

  // good
  useEffect(() => {
    // ...
  }, [])

  // 5.5 Additional destructures
  const { property } = something

  return (
    <div>
      {/* Separate elements if not closed on the same line to make the code clearer */}
      {/* bad */}
      <div>
        <div>
          <p>Lorem ipsum</p>
          <p>Pellentesque arcu</p>
        </div>
        <p>Lorem ipsum</p>
        <p>Pellentesque arcu</p>
      </div>
      <div>
        <p>
          Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
          arcu. Et harum quidem rerum facilis est et expedita distinctio.
        </p>
        <p>Pellentesque arcu</p>
        <p>
          Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Pellentesque
          arcu. Et harum quidem rerum facilis est et expedita distinctio.
        </p>
      </div>

      {/* good */}
      <Wrapper>
        <div>
          <p>Lorem ipsum</p>
          <p>Pellentesque arcu</p>
        </div>

        <p>Lorem ipsum</p>
        <p>Pellentesque arcu</p>
      </Wrapper>

      <div>
        <div>
          <p>
            Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
            Pellentesque arcu. Et harum quidem rerum facilis est et expedita
            distinctio.
          </p>

          <p>Pellentesque arcu</p>

          <p>
            Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
            Pellentesque arcu. Et harum quidem rerum facilis est et expedita
            distinctio.
          </p>
        </div>
      </div>
    </div>
  )
}

// 6. Exports
export { Component }
export type { ComponentProps }
```

- Use PropsWithChildren

```jsx
import React, { PropsWithChildren } from 'react'

type ComponentProps = {
  someProperty: string
}

// Good
function Component({
  someProperty,
  children
}: PropsWithChildren<ComponentProps>) {
  // ...
}
```

- Separate function from the JSX if it takes more than 1 line

```jsx
// Not recommend
<button
  onClick={() => {
    setState(!state);
    resetForm();
    reloadData();
  }}
/>

// Recommend
<button onClick={() => setState(!state)} />

// Recommend
const handleButtonClick = () => {
  setState(!state);
  resetForm();
  reloadData();
}

<button onClick={handleButtonClick} />
```

- Avoid using indexes as key props (according to the situation)

```jsx
// Bad
const List = () => {
  const list = ['item1', 'item2', 'item3']

  return (
    <ul>
      {list.map((value, index) => {
        return <li key={index}>{value}</li>
      })}
    </ul>
  )
}

// Good
const List = () => {
  const list = [
    { id: '111', value: 'item1' },
    { id: '222', value: 'item2' },
    { id: '333', value: 'item3' }
  ]

  return (
    <ul>
      {list.map(item => {
        return <li key={item.id}>{item.value}</li>
      })}
    </ul>
  )
}
```

- Use fragments

```jsx
// Not use
const ActionButtons = ({ text1, text2 }) => {
  return (
    <div>
      <button>{text1}</button>
      <button>{text2}</button>
    </div>
  )
}

// Use
const Button = ({ text1, text2 }) => {
  return (
    <>
      <button>{text1}</button>
      <button>{text2}</button>
    </>
  )
}
```

- Prefer destructuring properties

Prefer destructuring properties so it is clear what properties are used in the component.

```jsx
// Not really bad
const Button = props => {
  return <button>{props.text}</button>
}

// Good
const Button = props => {
  const { text } = props

  return <button>{text}</button>
}

// Good
const Button = ({ text }) => {
  return <button>{text}</button>
}
```

- Separation of concerns

Separating the business logic from the presentation (rendering) makes the components code more readable. Most of the time this is applicable for the page/screen/container components where you are about to use multiple hooks or useEffects. Then the final code starts to be huge enough to be unreadable.

- Custom hooks

```jsx
// Bad
const ScreenDimensions = () => {
  const [windowSize, setWindowSize] = useState({
    width: undefined,
    height: undefined
  })

  useEffect(() => {
    function handleResize() {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight
      })
    }

    window.addEventListener('resize', handleResize)
    handleResize()

    return () => window.removeEventListener('resize', handleResize)
  }, [])

  return (
    <>
      <p>Current screen width: {windowSize.width}</p>
      <p>Current screen height: {windowSize.height}</p>
    </>
  )
}

// Good
const useWindowSize = () => {
  const [windowSize, setWindowSize] = useState({
    width: undefined,
    height: undefined
  })

  useEffect(() => {
    function handleResize() {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight
      })
    }

    window.addEventListener('resize', handleResize)
    handleResize()

    return () => window.removeEventListener('resize', handleResize)
  }, [])

  return windowSize
}

const ScreenDimensions = () => {
  const windowSize = useWindowSize()

  return (
    <>
      <p>Current screen width: {windowSize.width}</p>
      <p>Current screen height: {windowSize.height}</p>
    </>
  )
}
```

- Avoid huge components

Whenever it is possible, split your component into smaller chunks. Often applicable when you are using conditional rendering or defining the columns for a data grid, etc. Also applicable when a lot of custom hooks are used — check the section above.

```jsx
// Bad
const SomeSection = ({ isEditable, value }) => {
  if (isEditable) {
    return (
      <Section>
        <Title>Edit this content</Title>
        <Content>{value}</Content>
        <Button>Clear content</Button>
      </Section>
    )
  }

  return (
    <Section>
      <Title>Read this content</Title>
      <Content>{value}</Content>
    </Section>
  )
}

// Good
const EditableSection = ({ value }) => {
  return (
    <Section>
      <Title>Edit this content</Title>
      <Content>{value}</Content>
      <Button>Clear content</Button>
    </Section>
  )
}

const DetailSection = ({ value }) => {
  return (
    <Section>
      <Title>Read this content</Title>
      <Content>{value}</Content>
    </Section>
  )
}

const SomeSection = ({ isEditable, value }) => {
  return isEditable ? (
    <EditableSection value={value} />
  ) : (
    <DetailSection value={value} />
  )
}
```

- Group the state whenever possible

```jsx
// Not recommend
const [username, setUsername] = useState('')
const [password, setPassword] = useState('')

// Recommend
const [user, setUser] = useState({})
```

- Use shorthand for boolean props

```jsx
// not recommend
<Form hasPadding={true} withError={true} />

// Recommend
<Form hasPadding withError />
```

- Avoid curly braces for string props

```jsx
// Bad
<Title variant={"h1"} value={"Home page"} />

// Good
<Title variant="h1" value="Home page" />
```

- Avoid using inline styles
  Ideally, use some third party for handling styles like CSS modules, JSS, styled-components, etc. or create a custom hook.

```jsx
// Bad
const Title = props => {
  return (
    <h1 style={{ fontWeight: 600, fontSize: '24px' }} {...props}>
      {children}
    </h1>
  )
}

// Good
const useStyles = props => {
  return useMemo(
    () => ({
      header: { fontWeight: props.isBold ? 700 : 400, fontSize: '24px' }
    }),
    [props]
  )
}

const Title = props => {
  const styles = useStyles(props)

  return (
    <h1 style={styles.header} {...props}>
      {children}
    </h1>
  )
}
```

- Prefer conditional rendering with ternary operator

```jsx
const { role } = user

// Bad
if (role === ADMIN) {
  return <AdminUser />
} else {
  return <NormalUser />
}

// Good
return role === ADMIN ? <AdminUser /> : <NormalUser />
```

- Use constants or enums for string values

```jsx

// Bad
if (role === 'admin') {
  return <AdminUser />;
}

// Good
enum Roles {
  admin = 'admin',
  basic = 'basic',
}

if (role === Roles.admin) {
  return <AdminUser />;

```

- Use type aliases

```jsx
export type TodoId = number
export type UserId = number

export interface Todo {
  id: TodoId;
  name: string;
  completed: boolean;
  userId: UserId;
}

export type TodoList = Todo[]
```

- Don’t use third-party libraries directly
  Instead of importing the third-party libraries directly, re-export them in one centralised place.

src/lib/store.ts

```tsx
export { useDispatch, useSelector } from 'react-redux'

src / lib / query.ts

export { useQuery, useMutation, useQueryClient } from 'react-query'
```

- Use descriptive variable names

```jsx
// Avoid single letter names
const n = 'Max'
// Recommend
const name = 'Max'

// Avoid abbreviations
const sof = 'Sunday'
// Recommend
const startOfWeek = 'Sunday'

// Avoid meaningless names
const foo = false
// Recommend
const appInit = false
```

- Avoid long list of function arguments

```jsx
// Not recommend
function createPerson(firstName, lastName, height, weight, gender) {
  // ...
}

// Recommend
function createPerson({ firstName, lastName, height, weight, gender }) {
  // ...
}

// Recommend
function createPerson(person) {
  const { firstName, lastName, height, weight, gender } = person
  // ...
}
```

- Use object destructuring

```jsx
// Not recommend
return (
  <>
    <div> {user.name} </div>
    <div> {user.age} </div>
    <div> {user.profession} </div>
  </>
)

// Recommend
const { name, age, profession } = user

return (
  <>
    <div> {name} </div>
    <div> {age} </div>
    <div> {profession} </div>
  </>
)
```

## Bug Avoidance

- Use optional chaining if things can be null

- Use the guard pattern/prop types/typescript to ensure your passed in parameters are valid

- Create PURE functions and avoid side-effects

- Avoid mutating state when working with arrays

- Remove all console.log()

- Treat props as read-only. Do not try to modify them

## Architecture & Clean Code

- No DRY violations. Create utility files to avoid duplicate code

- Follow the component/presentation pattern where appropriate. Components should follow the single responsibility principle

- Use Higher Order Components where appropriate

- Split code into respective files, JavaScript, test, and CSS

- Create a index.js within each folder for exporting. This will reduce repeating names on the imports

```jsx
import { Nav } from './Nav.tsx'
import { CookieBanner } from './CookieBanner.tsx'

export { Nav, CookieBanner }
```

- Only include one React component per file

- Favour functionless components

- Do not use mixins

- No unneeded comments

- Methods that are longer than the screen should be refactored into smaller units

- Commented out code should be deleted, not committed

## Testing

- Write tests

- Define a quality gate using coveralls

- Don't test more than one thing in a test

- No logic should exist within your test code

- Test classes only test one class

- Code that needs to talk to a network, or, database is mocked

## CSS&SASS

- Avoid Inline CSS

- A naming convention is defined and followed (BEM, SUIT, etc..) or React camelCased Property Names && Angular style encapsulation ([Angular style encapsulation](https://angular.io/guide/component-styles))\*\*

- Avoid using !important

- Consider Mobile first approach when you start to write your CSS

- React can be used to power animations. See React Transition Group, React Motion, React Spring, or Framer Motion, for example.

- Angular can you use Angular animation. The main Angular modules for animations are @angular/animations and @angular/platform-browser. When you create a new project using the CLI, these dependencies are automatically added to your project. ([Angular Animation](https://https://angular.io/guide/animations))\*\*

### Naming

- Use camelCase for object keys (i.e. "selectors").

  > Why? We access these keys as properties on the `styles` object in the component, so it is most convenient to use camelCase.

  ```js
  // bad
  {
    'bermuda-triangle': {
      display: 'none',
    },
  }

  // good
  {
    bermudaTriangle: {
      display: 'none',
    },
  }
  ```

- Use an underscore for modifiers to other styles.

  > Why? Similar to BEM, this naming convention makes it clear that the styles are intended to modify the element preceded by the underscore. Underscores do not need to be quoted, so they are preferred over other characters, such as dashes.

  ```js
  // bad
  {
    bruceBanner: {
      color: 'pink',
      transition: 'color 10s',
    },

    bruceBannerTheHulk: {
      color: 'green',
    },
  }

  // good
  {
    bruceBanner: {
      color: 'pink',
      transition: 'color 10s',
    },

    bruceBanner_theHulk: {
      color: 'green',
    },
  }
  ```

- Use `selectorName_fallback` for sets of fallback styles.

  > Why? Similar to modifiers, keeping the naming consistent helps reveal the relationship of these styles to the styles that override them in more adequate browsers.

  ```js
  // bad
  {
    muscles: {
      display: 'flex',
    },

    muscles_sadBears: {
      width: '100%',
    },
  }

  // good
  {
    muscles: {
      display: 'flex',
    },

    muscles_fallback: {
      width: '100%',
    },
  }
  ```

- Use a separate selector for sets of fallback styles.

  > Why? Keeping fallback styles contained in a separate object clarifies their purpose, which improves readability.

  ```js
  // bad
  {
    muscles: {
      display: 'flex',
    },

    left: {
      flexGrow: 1,
      display: 'inline-block',
    },

    right: {
      display: 'inline-block',
    },
  }

  // good
  {
    muscles: {
      display: 'flex',
    },

    left: {
      flexGrow: 1,
    },

    left_fallback: {
      display: 'inline-block',
    },

    right_fallback: {
      display: 'inline-block',
    },
  }
  ```

- Use device-agnostic names (e.g. "small", "medium", and "large") to name media query breakpoints.

  > Why? Commonly used names like "phone", "tablet", and "desktop" do not match the characteristics of the devices in the real world. Using these names sets the wrong expectations.

  ```js
  // bad
  const breakpoints = {
    mobile: '@media (max-width: 639px)',
    tablet: '@media (max-width: 1047px)',
    desktop: '@media (min-width: 1048px)'
  }

  // good
  const breakpoints = {
    small: '@media (max-width: 639px)',
    medium: '@media (max-width: 1047px)',
    large: '@media (min-width: 1048px)'
  }
  ```

### Ordering

- Define styles after the component.

  > Why? We use a higher-order component to theme our styles, which is naturally used after the component definition. Passing the styles object directly to this function reduces indirection.

  ```jsx
  // bad
  const styles = {
    container: {
      display: 'inline-block',
    },
  };

  function MyComponent({ styles }) {
    return (
      <div {...css(styles.container)}>
        Never doubt that a small group of thoughtful, committed citizens can
        change the world. Indeed, it’s the only thing that ever has.
      </div>
    );
  }

  export default withStyles(() => styles)(MyComponent);

  // good
  function MyComponent({ styles }) {
    return (
      <div {...css(styles.container)}>
        Never doubt that a small group of thoughtful, committed citizens can
        change the world. Indeed, it’s the only thing that ever has.
      </div>
    );
  }

  export default withStyles(() => ({
    container: {
      display: 'inline-block',
    },
  }))(MyComponent);
  ```

### Nesting

- Leave a blank line between adjacent blocks at the same indentation level.

  > Why? The whitespace improves readability and reduces the likelihood of merge conflicts.

  ```js
  // bad
  {
    bigBang: {
      display: 'inline-block',
      '::before': {
        content: "''",
      },
    },
    universe: {
      border: 'none',
    },
  }

  // good
  {
    bigBang: {
      display: 'inline-block',

      '::before': {
        content: "''",
      },
    },

    universe: {
      border: 'none',
    },
  }
  ```

### Inline

- Use inline styles for styles that have a high cardinality (e.g. uses the value of a prop) and not for styles that have a low cardinality.

  > Why? Generating themed stylesheets can be expensive, so they are best for discrete sets of styles.

  ```jsx
  // bad
  export default function MyComponent({ spacing }) {
    return (
      <div style={{ display: 'table', margin: spacing }} />
    );
  }

  // good
  function MyComponent({ styles, spacing }) {
    return (
      <div {...css(styles.periodic, { margin: spacing })} />
    );
  }
  export default withStyles(() => ({
    periodic: {
      display: 'table',
    },
  }))(MyComponent);
  ```

### Themes

- Use an abstraction layer such as [react-with-styles](https://github.com/airbnb/react-with-styles) that enables theming. _react-with-styles gives us things like `withStyles()`, `ThemedStyleSheet`, and `css()` which are used in some of the examples in this document._

> Why? It is useful to have a set of shared variables for styling your components. Using an abstraction layer makes this more convenient. Additionally, this can help prevent your components from being tightly coupled to any particular underlying implementation, which gives you more freedom.

- Define colors only in themes.

  ```js
  // bad
  export default withStyles(() => ({
    chuckNorris: {
      color: '#bada55',
    },
  }))(MyComponent);

  // good
  export default withStyles(({ color }) => ({
    chuckNorris: {
      color: color.badass,
    },
  }))(MyComponent);
  ```

- Define fonts only in themes.

  ```js
  // bad
  export default withStyles(() => ({
    towerOfPisa: {
      fontStyle: 'italic',
    },
  }))(MyComponent);

  // good
  export default withStyles(({ font }) => ({
    towerOfPisa: {
      fontStyle: font.italic,
    },
  }))(MyComponent);
  ```

- Define fonts as sets of related styles.

  ```js
  // bad
  export default withStyles(() => ({
    towerOfPisa: {
      fontFamily: 'Italiana, "Times New Roman", serif',
      fontSize: '2em',
      fontStyle: 'italic',
      lineHeight: 1.5,
    },
  }))(MyComponent);

  // good
  export default withStyles(({ font }) => ({
    towerOfPisa: {
      ...font.italian,
    },
  }))(MyComponent);
  ```

- Define base grid units in theme (either as a value or a function that takes a multiplier).

  ```js
  // bad
  export default withStyles(() => ({
    rip: {
      bottom: '-6912px', // 6 feet
    },
  }))(MyComponent);

  // good
  export default withStyles(({ units }) => ({
    rip: {
      bottom: units(864), // 6 feet, assuming our unit is 8px
    },
  }))(MyComponent);

  // good
  export default withStyles(({ unit }) => ({
    rip: {
      bottom: 864 * unit, // 6 feet, assuming our unit is 8px
    },
  }))(MyComponent);
  ```

- Define media queries only in themes.

  ```js
  // bad
  export default withStyles(() => ({
    container: {
      width: '100%',

      '@media (max-width: 1047px)': {
        width: '50%',
      },
    },
  }))(MyComponent);

  // good
  export default withStyles(({ breakpoint }) => ({
    container: {
      width: '100%',

      [breakpoint.medium]: {
        width: '50%',
      },
    },
  }))(MyComponent);
  ```

- Define tricky fallback properties in themes.

  > Why? Many CSS-in-JavaScript implementations merge style objects together which makes specifying fallbacks for the same property (e.g. `display`) a little tricky. To keep the approach unified, put these fallbacks in the theme.

  ```js
  // bad
  export default withStyles(() => ({
    .muscles {
      display: 'flex',
    },

    .muscles_fallback {
      'display ': 'table',
    },
  }))(MyComponent);

  // good
  export default withStyles(({ fallbacks }) => ({
    .muscles {
      display: 'flex',
    },

    .muscles_fallback {
      [fallbacks.display]: 'table',
    },
  }))(MyComponent);

  // good
  export default withStyles(({ fallback }) => ({
    .muscles {
      display: 'flex',
    },

    .muscles_fallback {
      [fallback('display')]: 'table',
    },
  }))(MyComponent);
  ```

- Create as few custom themes as possible. Many applications may only have one theme.

- Namespace custom theme settings under a nested object with a unique and descriptive key.

  ```js
  // bad
  ThemedStyleSheet.registerTheme('mySection', {
    mySectionPrimaryColor: 'green'
  })

  // good
  ThemedStyleSheet.registerTheme('mySection', {
    mySection: {
      primaryColor: 'green'
    }
  })
  ```

---

CSS puns adapted from [Saijo George](https://saijogeorge.com/css-puns/).


## Clean Code

- General rules
1. Follow standard conventions.
2. Keep it simple stupid. Simpler is always better. Reduce complexity as much as possible.
3. Boy scout rule. Leave the campground cleaner than you found it.
4. Always find root cause. Always look for the root cause of a problem.

-  Design rules
1. Keep configurable data at high levels.
2. Prefer polymorphism to if/else or switch/case.
3. Separate multi-threading code.
4. Prevent over-configurability.
5. Use dependency injection.
6. Follow Law of Demeter. A class should know only its direct dependencies.

-  Understandability tips
1. Be consistent. If you do something a certain way, do all similar things in the same way.
2. Use explanatory variables.
3. Encapsulate boundary conditions. Boundary conditions are hard to keep track of. Put the processing for them in one place.
4. Prefer dedicated value objects to primitive type.
5. Avoid logical dependency. Don't write methods which works correctly depending on something else in the same class.
6. Avoid negative conditionals.

-  Names rules
1. Choose descriptive and unambiguous names.
2. Make meaningful distinction.
3. Use pronounceable names.
4. Use searchable names.
5. Replace magic numbers with named constants.
6. Avoid encodings. Don't append prefixes or type information.

-  Functions rules
1. Small.
2. Do one thing.
3. Use descriptive names.
4. Prefer fewer arguments.
5. Have no side effects.
6. Don't use flag arguments. Split method into several independent methods that can be called from the client without the flag.

-  Comments rules
1. Always try to explain yourself in code.
2. Don't be redundant.
3. Don't add obvious noise.
4. Don't use closing brace comments.
5. Don't comment out code. Just remove.
6. Use as explanation of intent.
7. Use as clarification of code.
8. Use as warning of consequences.

-  Source code structure
1. Separate concepts vertically.
2. Related code should appear vertically dense.
3. Declare variables close to their usage.
4. Dependent functions should be close.
5. Similar functions should be close.
6. Place functions in the downward direction.
7. Keep lines short.
8. Don't use horizontal alignment.
9. Use white space to associate related things and disassociate weakly related.
10. Don't break indentation.

-  Objects and data structures
1. Hide internal structure.
2. Prefer data structures.
3. Avoid hybrids structures (half object and half data).
4. Should be small.
5. Do one thing.
6. Small number of instance variables.
7. Base class should know nothing about their derivatives.
8. Better to have many functions than to pass some code into a function to select a behavior.
9. Prefer non-static methods to static methods.

-  Tests
1. One assert per test.
2. Readable.
3. Fast.
4. Independent.
5. Repeatable.

-  Code smells
1. Rigidity. The software is difficult to change. A small change causes a cascade of subsequent changes.
2. Fragility. The software breaks in many places due to a single change.
3. Immobility. You cannot reuse parts of the code in other projects because of involved risks and high effort.
4. Needless Complexity.
5. Needless Repetition.
6. Opacity. The code is hard to understand.