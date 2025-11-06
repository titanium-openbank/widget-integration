# Titanium Open Finance Widget

Widget JavaScript para integrar conectividade bancária Open Finance em sua aplicação.

## Instalação

### Via CDN (Recomendado)

```html
<script type="module">
  import { TitaniumWidget } from 'https://cdn.titaniumplus.com.br/titanium-widget.es.js';
  
  const widget = new TitaniumWidget({ ... });
</script>
```

## Uso Básico

```javascript
import { TitaniumWidget } from './titanium-widget.es.js';

const widget = new TitaniumWidget({
  apiKey: 'sua-api-key', // requisitar para o time
  marketplaceId: 'marketplace-123', // requisitar para o time
  container: '#widget-container', // ou elemento DOM
  environment: 'sandbox', // ou 'production'
  userId: 'user-123', // id interno do usuario
  userProfile: 'business', // ou 'investor'
  userData: {
    name: 'João Silva',
    cpf: '12345678901',
    cnpj: '12345678000123', // obrigatório para 'business'
    email: 'joao@example.com'
  },
  onSuccess: (data) => console.log('Sucesso:', data),
  onError: (error) => console.error('Erro:', error),
  debug: true
});

// Inicializar e montar
await widget.initialize();
widget.mount();
```

## Configuração
> Para a utilização do widget é **necessário a liberação do dominio e criação de apikey** junto ao time da plataforma.

### Campos Obrigatórios

- `apiKey`: Chave API fornecida pela plataforma
- `container`: Seletor CSS ou elemento HTMLElement
- `environment`: `'sandbox'` ou `'production'`
- `userId`: ID único do usuário na plataforma
- `userProfile`: `'business'` (empresário) ou `'investor'` (investidor)
- `marketplaceId`: ID do marketplace
- `userData`: Dados do usuário (nome, CPF, CNPJ para business, email)

### Campos Opcionais

- `backendUrl`: URL do backend (usa padrão se não informado)
- `onSuccess`: Callback executado em operações bem-sucedidas
- `onError`: Callback executado em erros
- `onStateChange`: Callback para mudanças de estado
- `debug`: Ativa logs detalhados no console

## Perfis de Usuário

### Business (Empresário)
- Cria Payment Customer automaticamente
- Permite receber antecipação de recebíveis
- CNPJ obrigatório

### Investor (Investidor)
- Cria Payment Recipient após preencher dados bancários
- Permite receber investimentos
- CNPJ obrigatório para Automatic PIX

## Eventos

```javascript
widget.on('connected', (data) => {
  console.log('Conta conectada:', data);
});

widget.on('paymentCustomerCreated', (data) => {
  console.log('Payment Customer criado:', data);
});

widget.on('paymentRecipientCreated', (data) => {
  console.log('Payment Recipient criado:', data);
});

widget.on('consentGranted', (data) => {
  console.log('Consentimento autorizado:', data);
});
```

## Métodos Principais

- `initialize()`: Autentica e prepara o widget
- `mount()`: Monta a UI no container
- `unmount()`: Remove a UI do DOM
- `destroy()`: Limpa todos os recursos
- `grantConsent(consentRequestId)`: Autoriza um consent request

## Exemplo Completo

```html
<!DOCTYPE html>
<html>
<head>
  <title>Widget Example</title>
</head>
<body>
  <div id="widget-container"></div>

  <script type="module">
    import { TitaniumWidget } from './titanium-widget.es.js';

    const widget = new TitaniumWidget({
      apiKey: 'sua-api-key',
      container: '#widget-container',
      environment: 'sandbox',
      userId: 'user-123',
      userProfile: 'business',
      marketplaceId: 'marketplace-123',
      userData: {
        name: 'João Silva',
        cpf: '12345678901',
        cnpj: '12345678000123',
        email: 'joao@example.com'
      },
      onSuccess: (data) => {
        console.log('Operação concluída:', data);
      },
      onError: (error) => {
        console.error('Erro:', error.message);
      }
    });

    // Inicializar
    widget.initialize().then(() => {
      widget.mount();
    });
  </script>
</body>
</html>
```

## Suporte

Para dúvidas ou problemas, consulte a documentação completa ou entre em contato com o suporte.

## Versão

v0.1.0

