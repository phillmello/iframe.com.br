# Guia de como transitar informações via Iframe

Para transitar informações via iframes iremos utilizar a API [postMessage](https://developer.mozilla.org/pt-PT/docs/Web/API/Window/postMessage), esse método permite a configuração segura de origem cruzada, ou seja, iremos enfiar a informação do código renderizado da tag iframe para o navegador que está o renderizando.

Este é o arquivo que renderiza o iframe `src/index.html`, criamos uma escuta para que toda vez o evento do tipo `message` seja disparado ao receber o evento disparamos a propriedade `data` diretamente para o `dataLayer`

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

Este é o arquivo iframe que contém o formulário, nele adicionamos uma escuta para quando o famulário for submetido seja disparado através do método `postMessage` contido na propriedade `parent` 
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

