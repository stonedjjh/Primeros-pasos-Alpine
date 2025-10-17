### Directivas

## x-data
La directiva `x-data` se utiliza para definir un componente de Alpine.js y su estado inicial. Se coloca en un elemento HTML y acepta un objeto JavaScript que contiene las propiedades y métodos del componente.

la data que definamos con x-data sera accesible en el elemento html
en donde se defina y a sus hijos

```html
<div x-data="{ contador: 0 }">
    <button>Incrementar</button>
    <span></span>
</div>
```

Aqui la variable contador es accesible en la etiqueta de su definicion **<div>**  y todos sus hijos

## x-text
La directiva `x-text` se utiliza para enlazar texto dinámico a un elemento HTML. Se coloca en un elemento y acepta una expresión JavaScript que se evalúa y se muestra como texto dentro del elemento.

```html
<div x-data="{ contador: 0 }">
    <button>Incrementar</button>
    <span x-text="contador"></span>
</div>
```

En este ejemplo, el valor de `contador` se mostrará dentro del elemento `<span>`.

## x-on
La directiva `x-on` se utiliza para manejar eventos en Alpine.js. Se coloca en un elemento HTML y acepta un evento y una expresión JavaScript que se ejecuta cuando ocurre el evento.  

podemos usar el shorthand @ mas el evento que queremos usar ejemplo para click

x-on:click
@click

son equivalentes.

```html
<div x-data="{ contador: 0 }">
    <button @click="contador++">Incrementar</button>
    <span x-text="contador"></span>
</div>
```

En este ejemplo, cuando se hace clic en el botón, el valor de `contador` se incrementa en 1 y se actualiza automáticamente en el elemento `<span>`.

## x-for
La directiva `x-for` se utiliza para iterar sobre un arreglo en Alpine.

Se coloca en un elemento HTML y acepta una expresión que define la variable de iteración y el arreglo sobre el cual se itera.

```html
<div x-data="{ Lenguajes: ['C++', 'Java', 'Python','PHP']}">
    <p>Existen distintos lenguajes de programacion</p>
    <p>Estos son algunos de los mas usados en el backend: </p>
    <ul>
    <template x-for="Lenguaje in Lenguajes">
        <li>
            <span x-text="Lenguaje"></span>
        </li>
    </template>
    </ul>
</div>
```

## x-model
La directiva `x-model` se utiliza para enlazar el valor de un elemento de formulario (como un input, textarea o select) con una propiedad del estado del componente en Alpine.js. Se coloca en el elemento de formulario y acepta el nombre de la propiedad del estado a la que se desea enlazar.

```html
<div x-data="{ Lenguajes: ['C++', 'Java', 'Python','PHP'],
            otroLenguaje:''}">
    <p>Existen distintos lenguajes de programacion</p>
    <p>Estos son algunos de los mas usados en el backend: </p>
    <ul>
    <template x-for="Lenguaje in Lenguajes">
        <li>
            <span x-text="Lenguaje"></span>
        </li>
    </template>
    </ul>
    <input type="text" x-model="otroLenguaje" placeholder="Agrega otro lenguaje">
    <button @click="Lenguajes.push(otroLenguaje); otroLenguaje=''">
        Agregar
    </button>
</div>
```

Otra manera de hacer esto es definir funciones en el x-data
> [!TIP]
> para referirse a las variable usadas en el
> x-data dentro de una funcion se usa el this

```html
<div x-data="{ Lenguajes: ['C++', 'Java', 'Python','PHP'],
            otroLenguaje:'',
            agregarLenguaje(){
            this.Lenguajes.push(this.otroLenguaje);
            this.otroLenguaje='';
            }}">
    <p>Existen distintos lenguajes de programacion</p>
    <p>Estos son algunos de los mas usados en el backend: </p>
    <ul>
    <template x-for="Lenguaje in Lenguajes">
        <li>
            <span x-text="Lenguaje"></span>
        </li>
    </template>
    </ul>
    <input type="text" x-model="otroLenguaje" placeholder="Agrega otro lenguaje">
    <button @click="agregarLenguaje">
        Agregar
    </button>
</div>
```

## x-show
La directiva `x-show` se utiliza para mostrar o ocultar un elemento en Alpine.

Se coloca en un elemento HTML y acepta una expresión JavaScript que se evalúa como verdadera o falsa. Si la expresión es verdadera, el elemento se muestra; si es falsa, el elemento se oculta.

```html
<div id="externo" x-data="{ visible: false }">
    <p>Este es un elemento externo</p>
    <div id="interno" x-show="visible">
        <p>Este es un elemento interno y Oculto</p>
    </div>
    <button @click="visible=!visible">
        Mostrar/Ocultar
    </button>
</div>
```
## x-bind
**x-bind (alias ':')**

La directiva x-bind vincula atributos o propiedades del DOM a valores reactivos definidos en x-data. Permite que la vista se actualice automáticamente cuando cambian los datos, manteniendo la sincronía entre estado y representación sin manipulación manual del DOM.

Casos de uso principales:

- Enlace de valor (:value)
    • Asigna dinámicamente el valor de un input a la propiedad reactiva correspondiente.
    • Mantiene el campo sincronizado con el estado inicial y sus actualizaciones.
- Enlace de propiedad (disabled)
    • Vincula atributos booleanos como disabled a expresiones reactivas.
    • Permite habilitar o deshabilitar controles según el estado sin lógica imperativa.
- Enlace a estilos (style / color)
    • Actualiza propiedades de estilo (por ejemplo color) según valores reactivos.
    • Facilita cambios visuales declarativos en respuesta a cambios de estado.
Notas:
- x-bind acepta tanto la sintaxis larga (x-bind:attr) como el atajo ':' (ej. :value).

- Las vinculaciones actualizan el DOM de forma reactiva; si necesita control adicional.

```html
<div
    x-data="{ nombre: 'Joe Doe', 
        disabled: false,
        color: 'green' }"
    >
    <div class="form-row">
        <label for="nombre">Nombre</label>
        <input id="nombre" type="text" :value="nombre" :disabled="disabled" />
        <button type="button" @click="disabled = true">Desactivar</button>
        <p x-text="disabled ? 'Input desactivado' : 'Input activo'"></p>
        <button
            type="button"
            @click="color = color === 'green' ? 'red' : 'green'"
        >
            Cambiar de color
        </button>
        <!-- Caja que muestra el color; se enlaza el style al fondo usando :style -->
        <div
            class="color-box"
            :style="`background-color: ${color}; width: 120px; height: 36px; margin-top: .5rem; border-radius: 6px;`"
            aria-hidden="true"
        >
        </div>
    </div>
</div>
```
