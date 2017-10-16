
### What are decorators ?

Decorators are a stage 2 proposal for JavaScript and are already available as an experimental feature of TypeScript.

They are a form of metaprogramming, which is a distinct type of paradigm in which computer programs have the ability to treat programs as their data.

Being a feature similar to C#\`s attributes and Java`s annotations, they can be added to classes, methods, properties and parameters to enhance the type they are assigned to.


### Implementing custom decorators


Implementing a decorator is as simple as writing a function. Below is an example of a function decorator that marks a function inside a class as deprecated by logging a warning to the console.

```typescript
function deprecated (target: any, name: string, descriptor: PropertyDescriptor) {
	console.warn('This method is now deprecated!')
}

@deprecated
function markMe() { }
```

Decorators are commonly used as decorator factories. Decorator factories are just functions that wrap the decorators definition.  

```typescript
function deprecated2 (errorMessage: string) {
	return (target: any, name: string, descriptor: PropertyDescriptor) {
	console.warn(errorMessage);
}

@deprecated('This method is now deprecated!')
function markMe() { }
```

By wrapping the decorator definition they enable the ability for enhacing the decorators even more and kinda remind you of decorating a decorator.


### Different types of decorators 

Decorators can be categorized depending on the type they be assinged to and only vary on the number and type of the parameters.

**Function decorators**

```language:typescript
 functionDecorator (target: any, memberName: string, descriptor: PropertyDescriptor) { }
```

Function decorators accept three parameters. The first is either the constructor function for a static method or the prototype for the class if it is an instance method. The second parameter is the name of the decorated function and the third is the property descriptor.

**Class decorators**

```typescript
function sealed (target: Function) { 
	Object.seal(target);
    Object.seal(target.prototype);
}

@sealed
class MarkMe () { }
```

For the class decorator the first and only parameter is the constructor function for the class. They can return void or the type of the constructor making it possible to override the constructor. 

**Property decorators**

```typescript
function preventEnumeration (target: Object, propertyKey: string) { 
	let descriptor = Object.getOwnPropertyDescriptor(target, propertyKey) || {};
    if (descriptor.enumerable != value) {
  	    descriptor.enumerable = value;
  		Object.defineProperty(target, propertyKey, descriptor)
    }
}

@preventEnumeration
function markMe() { }

```

Property decorators accept two parameters. The first is the either the construction function or the class prototype and the second parameter is name of the decorated property.

**Parameter decorators**

```typescript
function logger (target: Object, propertyKey: string, parameterIndex: number) {
	console.log(target[propertyKey]);
}

function markMe (@logger msg: string) { }
```

Parameter decorators define three parameter. The first two parameters are the same as with property decorator and the third is the index in the parameter list stating with 0 for the first parameter.


### Summary

Decorators are an important feature for a language, enabling developer by writing programs in a more declarative style and hopefully they will be available in javascript soon.

