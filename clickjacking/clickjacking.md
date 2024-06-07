# ¿Clickjacking?

## <mark style="color:red;">¿Qué es el Clickjacking?</mark>

El Clickjacking, también conocido como "UI redressing" (reencuadre de la interfaz de usuario), es una técnica de ataque en la que un atacante engaña a un usuario para que haga clic en un elemento de la interfaz de usuario (UI) que es invisible o disfrazado como otro elemento. Esto permite al atacante hacer que el usuario realice acciones no deseadas, como cambiar configuraciones, enviar información confidencial, o realizar transacciones financieras sin el conocimiento o el consentimiento del usuario.

## <mark style="color:red;">¿Cómo funciona el Clickjacking?</mark>

El Clickjacking se basa en engañar al usuario mediante la manipulación de la interfaz gráfica de una página web. Esto generalmente se logra usando capas superpuestas y opacidad para ocultar elementos interactivos. Aquí está el proceso típico de un ataque de Clickjacking:

1. <mark style="color:red;">**Creación de una página maliciosa**</mark><mark style="color:red;">:</mark> El atacante crea una página web que incluye una iframe con el contenido legítimo que desea explotar.
2. <mark style="color:red;">**Superposición de la iframe**</mark><mark style="color:red;">:</mark> El iframe que contiene el contenido legítimo se superpone con elementos visuales que engañan al usuario. Por ejemplo, el atacante puede colocar botones, enlaces o formularios encima de los elementos de la iframe.
3. <mark style="color:red;">**Manipulación de la opacidad y el tamaño**</mark><mark style="color:red;">:</mark> El iframe puede ser hecho invisible (opacidad 0) o reducido a un tamaño tan pequeño que el usuario no puede verlo.
4. <mark style="color:red;">**Engaño al usuario**</mark><mark style="color:red;">:</mark> El usuario es atraído a la página maliciosa y engañado para que haga clic en lo que parece ser un botón o enlace legítimo. Sin embargo, en realidad está haciendo clic en un botón o enlace en la iframe oculta.

## <mark style="color:red;">Ejemplo de Clickjacking</mark>

Un ejemplo común de clickjacking es cuando un atacante quiere que un usuario haga clic en el botón de "Me gusta" de una red social. El atacante podría crear una página que se vea como un juego en línea, pero debajo del botón "Iniciar" del juego, hay un iframe invisible con el botón "Me gusta" de la red social.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Juego en línea</title>
    <style>
        iframe {
            position: absolute;
            top: 50px;
            left: 50px;
            width: 200px;
            height: 200px;
            opacity: 0; /* Hacer la iframe invisible */
            z-index: 2; /* Colocar la iframe encima del botón */
        }
        #fakeButton {
            position: absolute;
            top: 50px;
            left: 50px;
            width: 200px;
            height: 200px;
            background-color: blue;
            z-index: 1;
        }
    </style>
</head>
<body>
    <div id="fakeButton">Iniciar Juego</div>
    <iframe src="http://redsocial.com/like-button"></iframe>
</body>
</html>
```

## <mark style="color:red;">Mitigación del Clickjacking</mark>

Para protegerse contra ataques de clickjacking, se pueden implementar varias técnicas:

1. <mark style="color:red;">**Encabezados HTTP de Seguridad**</mark><mark style="color:red;">:</mark>
   *   <mark style="color:red;">**X-Frame-Options**</mark><mark style="color:red;">:</mark> Este encabezado puede usarse para indicar que un navegador no debe permitir que una página sea cargada dentro de un iframe. Sus valores pueden ser `DENY` (nunca permitir) o `SAMEORIGIN` (permitir solo si el iframe pertenece al mismo dominio).

       ```http
       X-Frame-Options: DENY
       ```

       ```http
       X-Frame-Options: SAMEORIGIN
       ```
   *   **Content Security Policy (CSP)**: La directiva `frame-ancestors` de CSP permite especificar de qué dominios se permiten iframes.

       ```http
       Content-Security-Policy: frame-ancestors 'self'
       ```
2. <mark style="color:red;">**Protección en el Lado del Cliente**</mark><mark style="color:red;">:</mark>
   *   **Frame Busting**: Usar scripts en el lado del cliente para evitar que la página sea cargada en un iframe.

       ```javascript
       if (top !== self) {
           top.location = self.location;
       }
       ```
3. **Uso de Técnicas Visuales**: Hacer que los elementos interactivos sean difíciles de ocultar o superponer, por ejemplo, usando capas visibles y bordes en los botones y formularios.

## <mark style="color:red;">Conclusión</mark>

El Clickjacking es una amenaza seria que puede tener consecuencias significativas para la seguridad y privacidad del usuario. Implementar medidas de mitigación efectivas es crucial para proteger las aplicaciones web y asegurar una experiencia segura para los usuarios. Entender la naturaleza de estos ataques y cómo prevenirlos es una parte esencial de la seguridad web moderna.
