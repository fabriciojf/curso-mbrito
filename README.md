# curso-mbrito

This doc is only a suggestion to resolve thymeleaf problem in MBrito Spring Boot Course. 

* [Youtube: Curso de Spring Boot: Criando uma aplicação Java Web](https://www.youtube.com/playlist?list=PL8iIphQOyG-DHLpEx1TPItqJamy08fs1D)

### Tag tg:block problem related by Erik Endler

Quando adiciono th:block  no detalhesEvento ou formEvento da o erro :

```error
An error happened during template parsing (template: "class path resource [templates/evento/formEvento.html]")
```

### Sugestion

Replace th:block to th:fragment

#### MensagemValidacao.html

* [mensagemValidacao.html](https://github.com/MichelliBrito/cursospringboot/blob/master/src/main/resources/templates/mensagemValidacao.html)

In the file mensagemValidacao.html replace this:

```html
<div class="alert alert-success alert-dismissible" role="alert" th:if="${not #strings.isEmpty(mensagem)}">	
	<span th:text="${mensagem}"></span>
</div>
```

For this:

```html
<div th:fragment="alert (type, message)" 
     th:assert="${!#strings.isEmpty(type) and !#strings.isEmpty(message)}"      
     th:classappend="'alert-' + ${type}" class="alert alert-dismissable" >
   <button type="button" class="close" data-dismiss="alert" aria-hidden="true">&times;</button>
   <span th:text="${message}"></span>
</div>
```

#### DetalhesEvento.html

* [detalhesEvento.html](https://github.com/MichelliBrito/cursospringboot/blob/master/src/main/resources/templates/evento/detalhesEvento.html)

In the file detalhesEvento.html replace this:

```html
<th:block th:include="mensagemValidacao"></th:block>
```

For this: 

```html
<div th:if="${flashMessage != null}">
    <div th:replace="mensagemValidacao :: alert (type=${flashType}, message=${flashMessage})"></div>
</div>
```

In this piece of code th:if="${flashMessage != null}" we verifying if flashMessage exists in this context, if exists, insert mensagemValidacao.html content in the current file.

#### EventoController.java

* [EventoController.java](https://github.com/MichelliBrito/cursospringboot/blob/master/src/main/java/com/eventosapp/controllers/EventoController.java)

In the file EventoController.java replace this:

```java
attributes.addFlashAttribute("mensagem", "Verifique os campos!");
```

For this

```java
attributes.addFlashAttribute("flashMessage", "Verifique os campos!");
attributes.addFlashAttribute("flashType", "danger");
```
Setting message type danger if you are using Bootstrap to set **Danger** colors.

And replace this:

```java
attributes.addFlashAttribute("mensagem", "Convidado adicionado com sucesso!");
```

For this:

```java
attributes.addFlashAttribute("flashMessage", "Convidado adicionado com sucesso!");
attributes.addFlashAttribute("flashType", "success");
```

Setting message type danger if you are using Bootstrap to set **Success** colors.

### Thanks

Thanks Michelli Brito for the excellent course about Spring Boot for beginners.

I hope this material helps in some way.
