### Documentação do Código JavaScript (app.js)

Este arquivo JavaScript controla a submissão de um formulário de contato usando AJAX e a API do FormSubmit. Ele é responsável por capturar os dados do formulário, enviá-los para o servidor sem recarregar a página e exibir mensagens de sucesso ou erro com base no resultado do envio.

#### 1. **Classe `FormSubmit`**

A classe `FormSubmit` encapsula toda a lógica de envio do formulário.

##### **Construtor `constructor(settings)`**

O construtor recebe um objeto de configuração (`settings`) e inicializa as seguintes propriedades:

- **this.settings**: Armazena as configurações fornecidas (seletor do formulário, seletor do botão, mensagens de sucesso/erro).
- **this.form**: Seleciona o formulário usando o seletor fornecido em `settings.form`.
- **this.formButton**: Seleciona o botão de envio usando o seletor fornecido em `settings.button`.
- **this.url**: Captura o atributo `action` do formulário, que contém o URL para onde os dados serão enviados.

##### **Exemplo:**

```javascript
constructor(settings) {
  this.settings = settings;
  this.form = document.querySelector(settings.form);
  this.formButton = document.querySelector(settings.button);
  if (this.form) {
    this.url = this.form.getAttribute("action");
  }
}
```

#### 2. **Métodos**

##### **`displaySuccess()`**

Este método é chamado quando a submissão do formulário é bem-sucedida. Ele substitui o conteúdo do formulário com a mensagem de sucesso definida nas configurações.

```javascript
displaySuccess() {
  this.form.innerHTML = this.settings.success;
}
```

##### **`displayError()`**

Este método é chamado em caso de falha na submissão do formulário. Ele substitui o conteúdo do formulário com a mensagem de erro definida nas configurações.

```javascript
displayError() {
  this.form.innerHTML = this.settings.error;
}
```

##### **`getFormObject()`**

Este método coleta todos os dados dos campos do formulário e os organiza em um objeto JavaScript. Ele percorre todos os campos que possuem o atributo `name` e atribui o valor de cada campo ao respectivo nome no objeto resultante.

```javascript
getFormObject() {
  const formObject = {};
  const fields = this.form.querySelectorAll("[name]");
  fields.forEach((field) => {
    formObject[field.getAttribute("name")] = field.value;
  });
  return formObject;
}
```

##### **`onSubmission(event)`**

Este método é acionado quando o botão de envio é clicado. Ele previne o comportamento padrão do formulário, desabilita o botão e altera o texto do botão para "Enviando..." para indicar que a submissão está em progresso.

```javascript
onSubmission(event) {
  event.preventDefault();
  event.target.disabled = true;
  event.target.innerText = "Enviando...";
}
```

##### **`sendForm(event)`**

Este é o método principal, responsável por enviar o formulário. Ele é executado de forma assíncrona e faz o seguinte:

1. Chama `onSubmission()` para desabilitar o botão e preparar a interface.
2. Usa o método `fetch()` para enviar os dados do formulário ao servidor.
3. Se o envio for bem-sucedido, chama `displaySuccess()`.
4. Se houver um erro, chama `displayError()`.

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

#### 3. **Inicialização do Formulário**

Após a criação da classe `FormSubmit`, o código inicializa uma nova instância passando as configurações do formulário, botão, e as mensagens de sucesso e erro. O método `init()` adiciona um event listener ao botão de envio para disparar o envio do formulário quando clicado.

```javascript
const formSubmit = new FormSubmit({
  form: "[data-form]",
  button: "[data-button]",
  success: "<h1 class='success'>Mensagem enviada!</h1>",
  error: "<h1 class='error'>Não foi possível enviar sua mensagem.</h1>",
});
formSubmit.init();
```

#### 4. **Método `init()`**

Este método adiciona o event listener ao botão de envio, vinculado ao método `sendForm`.

```javascript
init() {
  if (this.form) this.formButton.addEventListener("click", this.sendForm);
  return this;
}
```

#### 5. **Resumo do Fluxo de Execução**

1. O usuário preenche o formulário e clica no botão "Enviar".
2. O método `sendForm` é chamado e desabilita o botão, alterando seu texto para "Enviando...".
3. Os dados do formulário são enviados via `fetch()` em formato JSON para o servidor do FormSubmit.
4. Se o envio for bem-sucedido, a mensagem de sucesso é exibida. Se houver falha, uma mensagem de erro é mostrada.