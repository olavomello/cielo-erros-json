# cielo-erro-json
JSON atualizado de retornos de erro de transações da CIELO

Para quem utiliza integrações com a CIELO é fundamental entender os erros gerados em algumas transações.
Este JSON foi pensado para isto. Ao receber o erro da API, utilize o retorno `returnCode` para entender qual o erro ocorrido que acabou gerando a negatica da transação.

<u>Dica:</u> Fique sempre atento ás negativas de pagamento realizadas em seu projeto. Muitas das negativas são compras autênticas geradas por <u>políticas internas</u> dos gateways de pagamento. Por isso fique de olho e não perca conversões importantes ;)

Se precisar estou logo ali, no <a href="https://www.linkedin.com/in/olavo-mello/" target="_blank">Linkedin</a>

## Códigos exemplo em : <b>NodeJs, PHP, Python, ReactJs, Rust, Lua</b>

### NodeJS
```nodejs
  const errorMessages = require('./cielo-erros.json');

  function getErrorMessage(returnCode) {
    const error = errorMessages.find(e => e.returnCode === returnCode);
    if (error) {
      return error.message;
    } else {
      return 'Unknown error';
    }
  }
```

### PHP
```php
  function getErrorMessage($returnCode) {
    $errorMessages = json_decode(file_get_contents('./cielo-erros.json'), true);
    foreach ($errorMessages as $error) {
      if ($error['returnCode'] === $returnCode) {
        return $error['message'];
      }
    }
    return 'Unknown error';
  }
```

### Python
```phyton
  import json

  def get_error_message(return_code):
      with open('./cielo-erros.json') as f:
          error_messages = json.load(f)
      for error in error_messages:
          if error['returnCode'] == return_code:
              return error['message']
      return 'Unknown error'
```

### ReactJs
```reacjs
  import errorMessages from './errorMessages.json';

  function getErrorMessage(returnCode) {
    const errorMessage = errorMessages.find((error) => error.returnCode === returnCode);
    return errorMessage ? errorMessage.message : 'An unknown error occurred.';
  }
```

### Rust
```rust
  use serde_json::{Result, Value};

  fn get_error_message(return_code: u32) -> String {
      let json_str = include_str!("errorMessages.json");
      let json: Value = serde_json::from_str(json_str).expect("Failed to deserialize JSON");
      for error in json.as_array().expect("JSON must be an array") {
          let error_obj = error.as_object().expect("JSON array must contain objects");
          if let Some(code) = error_obj.get("returnCode") {
              if let Some(code_int) = code.as_u64() {
                  if code_int == return_code as u64 {
                      if let Some(message) = error_obj.get("message") {
                          if let Some(message_str) = message.as_str() {
                              return message_str.to_string();
                          }
                      }
                  }
              }
          }
      }
      return "An unknown error occurred.".to_string();
  }
```

### Lua
```lua
  local function getErrorMessage(returnCode)
    local file = io.open('errorMessages.json', 'r')
    local json_str = file:read('*all')
    file:close()

    local json = require('cjson').decode(json_str)
    for _, error in ipairs(json) do
      if error.returnCode == returnCode then
        return error.message
      end
    end
    return 'An unknown error occurred.'
  end
```