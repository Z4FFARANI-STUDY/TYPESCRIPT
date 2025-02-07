# ANOTAÇÕES

`servidor-api`:
Servidor back-end criado com Express para demonstrar `fetch` de dados em outro servidor.

<br>

`tsconfig.json`:

- **compilerOptions** | Define opções de compilação para o TypeScript.
  - **outDir** | Especifica o diretório de saída para os arquivos JavaScript compilados.
  - **target** | Define a versão do ECMAScript para a qual o TypeScript será compilado (neste caso, ES6).
  - **noEmitOnError** | Se verdadeiro, impede a emissão de arquivos JavaScript se houver erros de compilação.
  - **noImplicitAn** | Se verdadeiro, gera um erro quando o tipo `any` é implicitamente assumido.
  - **removeComments** | Se verdadeiro, remove comentários dos arquivos JavaScript compilados.
  - **strictNullChecks** | Se verdadeiro, habilita verificações estritas de nulidade, melhorando a segurança de tipos.
  - **experimentalDecorators** | Habilita o suporte a decorators, que são uma forma especial de declarar funções que podem ser aplicadas a classes, métodos, acessores, propriedades ou parâmetros.
  - **sourceMap** | Usada para gerar arquivos de mapa de origem `.map` que facilitam a depuração do código TypeScript compilado.

- **include** | Especifica os arquivos e diretórios a serem incluídos na compilação (neste caso, todos os arquivos dentro de `app.ts` e seus subdiretórios).

<br>

## TIPAGEM
Permite definir tipos para variáveis, parâmetros de função, retornos de função e propriedades de classe, ajudando a evitar erros em tempo de desenvolvimento e melhorar a legibilidade do código.

```ts
let nome: string = "Alice";
let idade: number = 30;
let ativo: boolean = true;
```

```ts
function soma(a: number, b: number): number {
    return a + b;
}
```

```ts
class Pessoa {
    nome: string;
    idade: number;

    constructor(nome: string, idade: number) {
        this.nome = nome;
        this.idade = idade;
    }
}
```

<br>

## GENERICS
Permitem criar componentes reutilizáveis que funcionam com vários tipos. Eles são especialmente úteis para estruturas de dados como listas e pilhas.

```ts
class Box<T> {
    content: T;
    constructor(content: T) {
        this.content = content;
    }
}
```

<br>

## PUBLIC | PRIVATE | PROTECTED
Modificadores de acesso que controlam a visibilidade dos membros da classe.

- `public` | Acessível de qualquer lugar.
- `private` | Acessível apenas dentro da classe onde foi definido.
- `protected` | Acessível dentro da classe e subclasses.

```ts
class Example {
    public publicProperty: string;
    private privateProperty: string;
    protected protectedProperty: string;
}
```

<br>

## ENUMS
Permitem definir um conjunto de valores nomeados. São úteis para representar um conjunto fixo de constantes.

```ts
enum Color {
    Red,
    Green,
    Blue
}
```

<br>

## READONLY
Define que uma propriedade só pode ser atribuída durante a inicialização ou no construtor.

```ts
class Example {
    readonly name: string;
    constructor(name: string) {
        this.name = name;
    }
}
```

<br>

## STATIC
Define membros da classe que são acessíveis sem instanciar a classe.

```ts
class Example {
    static staticProperty: string = "static value";
}
```

<br>

## ABSTRAÇÃO
Abstração permite definir classes base com métodos abstratos que devem ser implementados por subclasses.

```ts
abstract class Animal {
    abstract makeSound(): void;
}
```

<br>

• `template` | Método abstrato que deve ser implementado pelas subclasses para retornar uma string de template.

```ts
abstract class View<T> {
    protected abstract template(model: T): string;
}
```

<br>

## HERANÇA
Herança permite que uma classe derive de outra, herdando suas propriedades e métodos.

```ts
class Dog extends Animal {
    makeSound() {
        console.log("Woof!");
    }
}
```

<br>

## RECUSA DE RETORNO `NULL`
Permite afirmar que um valor não é null ou undefined usando a asserção de tipo.

```ts
const elemento = document.querySelector('selector') as HTMLElement;
```

<br>

## DECORATOR
Decorator é uma forma especial de declarar funções que pode ser aplicada a classes, métodos, propriedades ou parâmetros. Ele permite adicionar comportamentos adicionais de forma declarativa. A ordem dos decorators tem diferença.

### Parâmetros do Decorator

- `target: any`: O alvo ao qual o decorator é aplicado. Pode ser o protótipo da classe para métodos de instância ou a própria classe para métodos estáticos.
- `propertyKey: string`: O nome da propriedade do método decorado.
- `descriptor: PropertyDescriptor`: O descritor de propriedade do método decorado, que permite modificar o comportamento do método.

### Decorator de método

```ts
export function escapar(
    target: any,
    propertyKey: string,
    descriptor: PropertyDescriptor
) {
    const metodoOriginal = descriptor.value;
    descriptor.value = function(...args: any[]) {
        let retorno = metodoOriginal.apply(this, args);
        if (typeof retorno === 'string') {
            console.log(`@escapar em ação na classe ${this.constructor.name} para o método ${propertyKey}`);
            retorno = retorno.replace(/<script>[\s\S]*?<\/script>/, '');
        }
        return retorno;
    }

    return descriptor;
}
```

### Exemplo

```ts
class Exemplo {
    @escapar
    metodo(): string {
        return '<script>alert("Olá!");</script>';
    }
}
```

### Decorator de propriedade
Decorators de propriedade são usados para adicionar comportamento ou metadados a propriedades de uma classe. Eles não recebem um descriptor como os decorators de método.

```ts
function logarPropriedade(target: any, propertyKey: string) {
    console.log(`Propriedade ${propertyKey} foi decorada`);
}

class Exemplo {
    @logarPropriedade
    propriedade: string;
}
```

### Exemplo

```ts
class Exemplo {
    @logarPropriedade
    propriedade: string = 'valor';
}
```

### Resumo

• **Decorators de método** | Modificam o comportamento de métodos.

• **Decorators de propriedade** | Adicionam metadados ou comportamento adicional a propriedades.

<br>

## INTERFACE
Interfaces em TypeScript são uma forma poderosa de definir contratos dentro do seu código. Elas permitem que você defina a estrutura de objetos, classes e funções, garantindo que eles sigam um formato específico. Classes aceitam múltiplas interfaces. Interfaces aceitam múltiplas interfaces. Em resumo, são uma ferramenta essencial em TypeScript para garantir a consistência e a segurança de tipos no seu código.

### Definição
Uma interface define a forma que um objeto deve ter. Ela pode incluir propriedades e métodos, mas não implementações.

```ts
interface Pessoa {
    nome: string;
    idade: number;
    saudar(saudacao: string): void;
}
```

### Implementação
Classes podem implementar interfaces para garantir que sigam a estrutura definida.

```ts
class Usuario implements Pessoa {
    constructor(public nome: string, public idade: number) {}

    saudar(saudacao: string): void {
        console.log(`${saudacao}, meu nome é ${this.nome}`);
    }
}
```

### Uso de interface com objetos
Interfaces também podem ser usadas para definir a forma de objetos literais.

```ts
const pessoa: Pessoa = {
    nome: 'Alice',
    idade: 30,
    saudar(saudacao: string) {
        console.log(`${saudacao}, meu nome é ${this.nome}`);
    }
};
```

### Interfaces para funções
Interfaces podem definir a assinatura de funções.

```ts
interface FuncaoSaudar {
    (nome: string, idade: number): string;
}

const saudar: FuncaoSaudar = (nome, idade) => {
    return `Olá, meu nome é ${nome} e eu tenho ${idade} anos.`;
};
```

### Herança de interfaces
Interfaces podem estender outras interfaces, permitindo a reutilização de contratos.

```ts
interface Animal {
    nome: string;
    mover(distancia: number): void;
}

interface Mamifero extends Animal {
    amamentar(): void;
}
```

<br>

## Polimorfismo
Polimorfismo é um princípio da programação orientada a objetos que permite que classes diferentes sejam tratadas de forma uniforme através de uma interface ou classe base comum. Em TypeScript, isso pode ser implementado usando interfaces ou classes base.

### Exemplo com interfaces
Define uma interface comum que diferentes classes implementam.

```typescript
interface Forma {
    area(): number;
}

class Circulo implements Forma {
    constructor(private raio: number) {}

    area(): number {
        return Math.PI * this.raio * this.raio;
    }
}

class Retangulo implements Forma {
    constructor(private largura: number, private altura: number) {}

    area(): number {
        return this.largura * this.altura;
    }
}

function imprimirArea(forma: Forma) {
    console.log(`Área: ${forma.area()}`);
}

const circulo = new Circulo(5);
const retangulo = new Retangulo(10, 5);

imprimirArea(circulo);    // Área: 78.53981633974483
imprimirArea(retangulo);  // Área: 50
```

### Exemplo com Classes Base
Define uma classe base abstrata que diferentes classes estendem.

```ts
abstract class Forma {
    abstract area(): number;
}

class Circulo extends Forma {
    constructor(private raio: number) {
        super();
    }

    area(): number {
        return Math.PI * this.raio * this.raio;
    }
}

class Retangulo extends Forma {
    constructor(private largura: number, private altura: number) {
        super();
    }

    area(): number {
        return this.largura * this.altura;
    }
}

function imprimirArea(forma: Forma) {
    console.log(`Área: ${forma.area()}`);
}

const circulo = new Circulo(5);
const retangulo = new Retangulo(10, 5);

imprimirArea(circulo);    // Área: 78.53981633974483
imprimirArea(retangulo);  // Área: 50
```