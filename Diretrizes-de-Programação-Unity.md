<h1>Diretrizes de Programação Unity</h1>

Aqui são descritas nossas diretrizes de programação, são boas práticas e orientações para se levar em conta enquanto estiver escrevendo seu código.

- [Geral](#geral)
  + [Sempre adicione modificadores de acesso explicitamente](#sempre-adicione-modificadores-de-acesso-explicitamente)
  + [Sempre mantenha os campos das classes como privados](#sempre-mantenha-os-campos-das-classes-como-privados)
  + [Evite usar valores "hard-coded"](evite-usar-valores-"hard-coded")
  + [Evite o uso das tags da Unity"](evite-o-uso-das-tags-da-Unity)  
  
- [Convenções de Nomeclatura]()
   + [Planilha de nomes]()
   + [Sempre escreva o código em inglês]()
   + [Sempre use palavras afirmativas para variáveis e funções booleanas]()
   + [Evite abreviações]()
   
- [Organização dos scripts]()
	+ [TO-DO]()
	+ [TO-DO]()

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

## Sempre mantenha os campos das classes como privados

Campos privados ajudam a manter o encapsulamento e a coesão das classes. Quando for necessário que eles sejam acessados de outras classes devemos utilizar Properties Get() Set() ou acesso através de métodos públicos.

Se a intenção for apenas de acessar o campo através do inspector, o campo deve se mantar privado com a adição do [SerializeField] antes do nome.

## Evite usar valores "hard-coded"

Considere usar uma variável declarada 

```csharp

//correto 

private const float _earthDiamater = 12.742f;
private float _currentEarthSize;

public void DoubleEarthDiamater()
{
	_currentEarthSize = _earthDiameter * 2;
}

//errado

private float _currentEarthSize;

public void DoubleEarthDiamater()
{
	_currentEarthSize = 12.742f * 2;
}

```

## Evite o uso das tags da Unity

O uso de tags favorece conflitos de merge e dificulta o debug do código.
Ao invés disso utilize Interfaces ou Monobehaviours exclusivos para Tags, nesse caso, coloque o sufixo Tag no nome da classe.

Ex: 
```csharp
public class AquaticEnemyTag : MonoBehaviour
```

WIP - Este documento está em construção.
