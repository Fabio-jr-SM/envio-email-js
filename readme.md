### Documentação do Código de Envio de E-mail Usando a API Ajax do FormSubmit

Este projeto implementa um formulário de contato que envia e-mails utilizando a API do FormSubmit por meio de requisições AJAX. O envio é gerenciado de forma assíncrona com JavaScript, evitando o recarregamento da página e oferecendo feedback ao usuário.

#### 1. Estrutura do Projeto

O projeto consiste em três arquivos principais:

- **index.html**: Contém o formulário e a estrutura básica do HTML.
- **style.css**: Estiliza o formulário e a página de contato.
- **app.js**: Implementa a lógica de envio do formulário via AJAX.

#### 2. **HTML (index.html)**

O arquivo HTML contém um formulário com campos para **nome**, **e-mail**, e **mensagem**, que será enviado para o endereço de e-mail especificado no atributo `action`.

```html
<form action="https://formsubmit.co/ajax/[email]" method="POST" data-form>
  <label for="nome">Nome</label>
  <input type="text" name="nome" id="nome" required />
  <label for="email">E-mail</label>
  <input type="email" name="email" id="email" required />
  <label for="mensagem">Mensagem</label>
  <textarea name="mensagem" id="mensagem" required></textarea>
  <button type="submit" data-button>Enviar</button>
</form>
```

- **action**: Define o URL do FormSubmit, que processa o envio.
- **method="POST"**: Especifica o método HTTP utilizado no envio.
- **data-form e data-button**: Utilizados para selecionar o formulário e o botão via JavaScript.

#### 3. **CSS (style.css)**

O CSS estiliza o formulário, criando uma aparência moderna e responsiva.

Principais estilizações:

- Definição de margens e espaçamento entre os elementos.
- Aplicação de transições suaves ao focar ou passar o mouse sobre campos de entrada.
- Cores personalizadas de fundo, texto, e botões para melhorar a usabilidade.

```css
input, textarea, button {
  width: 100%;
  padding: 1.25rem;
  font-weight: 700;
}
input:hover, input:focus, textarea:hover, textarea:focus {
  border-color: #1d90f5;
  box-shadow: 0 0 0 0.3125rem #26344a;
}
button {
  background: #1d90f5;
  transition: 0.3s;
}
```

#### 4. **JavaScript (app.js)**

O JavaScript controla o envio do formulário por AJAX utilizando a API do `fetch`. Ele captura os dados do formulário, converte-os em JSON e os envia para o FormSubmit.

##### Principais funcionalidades:

- **displaySuccess()**: Exibe uma mensagem de sucesso após o envio.
- **displayError()**: Exibe uma mensagem de erro em caso de falha no envio.
- **getFormObject()**: Converte os campos do formulário em um objeto JavaScript.
- **sendForm()**: Envia o formulário por meio de uma requisição `POST` para a API do FormSubmit.

```javascript
async sendForm(event) {
  try {
    this.onSubmission(event);
    await fetch(this.url, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        Accept: "application/json",
      },
      body: JSON.stringify(this.getFormObject()),
    });
    this.displaySuccess();
  } catch (error) {
    this.displayError();
  }
}
```

##### Instância do FormSubmit:

```javascript
const formSubmit = new FormSubmit({
  form: "[data-form]",
  button: "[data-button]",
  success: "<h1 class='success'>Mensagem enviada!</h1>",
  error: "<h1 class='error'>Não foi possível enviar sua mensagem.</h1>",
});
```

- **form**: Seleciona o formulário pelo atributo `data-form`.
- **button**: Adiciona um listener no botão para disparar o envio.
- **success e error**: Mensagens exibidas ao usuário após o envio ou em caso de erro.

#### 5. Fluxo de Envio

1. O usuário preenche o formulário.
2. Ao clicar em "Enviar", o JavaScript intercepta a submissão, serializa os dados e envia-os via AJAX.
3. O servidor do FormSubmit processa os dados e, se bem-sucedido, o usuário vê a mensagem "Mensagem enviada!". Se houver erro, é exibida uma mensagem de falha.

#### 6. Requisitos e Personalização

- **API FormSubmit**: Usada para o envio de e-mails sem a necessidade de um backend personalizado.
- **AJAX**: As requisições são enviadas sem recarregar a página.
- **Personalização**: Alterar os valores do atributo `action` no HTML para seu e-mail e modificar as mensagens de sucesso/erro conforme necessário.

