<h1>Diretrizes de Programação Unity</h1>

Aqui são descritas nossas diretrizes de programação, são boas práticas e orientações para se levar em conta enquanto estiver escrevendo seu código.

- [Geral](#geral)
  - [Sempre adicione modificadores de acesso explicitamente](#sempre-adicione-modificadores-de-acesso-explicitamente)

---
# Geral

## Sempre adicione modificadores de acesso explicitamente
Por padrão a Unity considera campos sem modificadores como sendo privados, para clarificar e padronizar nossos códigos, sempre iremos declarar os modificadores, mesmo para campos privados.

Sem o modificador de acesso sua variável ou método pode ser confundida com uma variável local ou com uma função local.

```csharp
// correto
private void DoSomething() {...}

// errado
void DoSomething() {...}
```

WIP - Este documento está em construção
