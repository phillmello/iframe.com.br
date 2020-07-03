Guia de como transitar informações via Iframe

Para transitar informações via iframes iremos utilizar a API [postMessage](https://developer.mozilla.org/pt-PT/docs/Web/API/Window/postMessage), esse método permite a configuração segura de origem cruzada, ou seja, iremos enfiar a informação do código renderizado da tag iframe para o navegador que está o renderizando.
`src/index.html` 
``` html
 <iframe src="iframe.html"></iframe>
    <script>
        var dataLayer = window.dataLayer || [];
        window.addEventListener("message", receiveMessage, false);

        function receiveMessage(event) {
            if (event.origin !== "http://localhost:8080") // Verificando dominio
                return;
            
            dataLayer.push(event.data)
        }
    </script>
```
``` html
    <form action="POST" id="myForm" onsubmit="">
        <input type="text" name="email" />
        <input type="submit" value="Cadastre-se na newsletter" />
    </form>


    <script type="text/javascript">
		document.getElementById("myForm").onsubmit = (e) => {
            e.preventDefault()
            parent.postMessage({"event": 'test'}, "http://localhost:8080")
        }

    </script>
```

Quando efetuamos o submit de um form por via de iframe temos que captar o evento via dataLayer, com um evento personalizado. 
Fazendo que os 2 navegadores se conversem o do iframe e o da url acessada.
Assim que demos submit no formulário faremos com que o navegador do iframe envie uma mensagem com o evento, para o navegador da página que estamos acessando.
O navegador interpreta e válida a menssagem e cria o evento personalizado via dataLayer com o método addEventListener e assim conseguimos implementar no gtm.
