Evento personalizado para iframes

Quando efetuamos o submit de um form por via de iframe temos que captar o evento via dataLayer, com um evento personalizado. 
Fazendo que os 2 navegadores se conversem o do iframe e o da url acessada.
Assim que demos submit no formulário faremos com que o navegador do iframe envie uma mensagem com o evento, para o navegador da página que estamos acessando.
O navegador interpreta e válida a menssagem e cria o evento personalizado via dataLayer com o método addEventListener e assim conseguimos implementar no gtm.
