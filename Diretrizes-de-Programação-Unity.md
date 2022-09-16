<h1>Diretrizes de Programação Unity</h1>

Aqui são descritas nossas diretrizes de programação, são boas práticas e orientações para se levar em conta enquanto estiver escrevendo seu código.

- [Geral](#geral)
  + [Sempre adicione modificadores de acesso explicitamente](#sempre-adicione-modificadores-de-acesso-explicitamente)
  + [Sempre mantenha os campos das classes como privados](#sempre-mantenha-os-campos-das-classes-como-privados)
  + [Evite usar valores "hard-coded"](#evite-usar-valores-"hard-coded")
  + [Evite o uso das tags da Unity"](#evite-o-uso-das-tags-da-Unity)  
  
- [Convenções de Nomeclatura](#convenções-de-nomeclatura)
   + [Planilha de nomes](#planilha-de-nomes)
   + [Sempre escreva o código em inglês](#sempre-escreva-o-código-em-inglês)
   + [Sempre use palavras afirmativas para variáveis e funções booleanas](#sempre-use-palavras-afirmativas-para-variáveis-e-funções-booleanas)
   + [Evite abreviações](#evite-abreviações)
   
- [Organização dos scripts](#organização-dos-scripts)
	+ [Ordem dos membros](#ordem-dos-membros)
	+ [Ordem de acessibilidade](ordem-de-acessibilidade)
	+ [Ordem dos métodos](ordem-dos-métodos)
	+ [Declare apenas uma variável por linha](declare-apenas-uma-variável-por-linha)
	+ [Nunca divida seu código em Regions](nunca-divida-seu-código-em-regions)
  - [Sempre coloque auto-properties na mesma linha](#sempre-coloque-auto-properties-na-mesma-linha)
  - [Considere o agrupamento de multiplos `for`, `foreach`, `using` e `while` statements](#considere-o-agrupamento-de-multiplos-for-foreach-using-e-while-statements)

- [Assets utilizados no cotidiano desenvolvidos pela equipe](#assets)
	+ [Coletânea de scripts Drag and Drop](#coletânea-de-scripts-drag-and-drop)
		+ [Drag Controller Script](#drag-controller-script)
		+ [Layer Script](#layer-script)
		+ [Configuração Drag And Drop](#configuração-drag-and-drop)

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

# Convenções de Nomeclatura

## Planilha de nomes


| Identificadores | Casing | Formatação | Exemplos | Observações |
| :--- | :--- | :--- | :--- | :--- |
| Class<br/> Struct<br/> Property<br/> Constants<br/> Readonly | PascalCase | {Nome} | `Player`<br/> `AIState`<br/> `PlayerController`<br/> `DefaultSpeed`<br/> `InitialSpeed` 
| Abstract Class | PascalCase | {Nome}Base | `CharacterBase` | - |
| Tests Class | PascalCase | {Tipo}Tests | `PlayerTests`<br/>`PrefabUtilityTests` | Dado o exemplo, é esperado que exista no projeto uma classe chamada `Player` e uma classe chamada `PrefabUtility`. |
| Extenções de Classes | PascalCase | {Tipo}Extensions | `TransformExtensions`<br/>`RigidbodyExtensions` | - |
| Delegates | PascalCase | {[NomeDoEvento]}`Handler` | `ButtonClickHandler` | - |
| Enums | PascalCase | {Nome} | `GameMode`<br/> `DisplayOptions` | |
| Interfaces | PascalCase | `I`{Adjetivo}<br/> `I`{[NomeDoEvento]}`Handler` | `IDamageable`<br/> `IDamageTakenHandler` | - |
| Argumentos Genéricos | PascalCase | `T`<br/> `T`{Nome}<br/> | `List<T>`<br/> `Dictionary<TKey, TValue>` | Se há apenas um argumento, use `T` como o nome. |
| C# event<br/> Método para invocar um evento | PascalCase | `On`{[NomeDoEvento]} | `OnButtonClick`<br/> `OnButtonClick()` |  |
| Método para escutar um evento | PascalCase | `Handle`{[NomeDoEvento]} | `HandleButtonClick()` | Esses métodos estão diretamente ligados à execução de um evento. |
| Método | PascalCase | {Ação} | `Jump()` | - |
| Métodos Assincronos | PascalCase | {Ação}`Async` | `DownloadFileAsync()` | - |
| Métodos de Corrotina | PascalCase | {Ação}`Coroutine` | `DownloadFileCoroutine()` | - |
| Campos públicos<br/> Campos constantes<br/>Campos somente leitura | PascalCase | {Nome} | `SpeedRange` | Apenas structs devem ter campos não-privados que não são constantes ou somente leitura. |
| Campos privados | camelCase | `_`{nome} | `_speedRange` | - |
| Parâmetros<br/> Variáveis locais | camelCase | {nome} | `speedRange` | - |

## Sempre escreva o código em inglês

Inglês é o idioma internacional para código. Por uma questão de padronização, todo o nosso código será escrito em inglês.

## Sempre use palavras afirmativas para variáveis e funções booleanas

Usar palavras afirmativas evita confusões de interpretação ao ler e utilizar o código. Devemos por exemplo usar _isReady ao invés de _isNotReady.

## Evite abreviações

De preferência por utilizar os nomes completos ao invés de abreviações, isso deixa o código mais claro e legível.

# Organização dos scripts

## Ordem de acessibilidade

Quanto mais exposto é o membro, maior a sua prioridade.

- `public`
- `internal`
- `protected internal`
- `protected`
- `private protected`
- `private`
 
## Ordem dos métodos

A ordem dos métodos deve seguir a seguinte lista:

- Unity callbacks
  - Reset()
  - OnValidate()
  - Awake()
  - OnEnable()
  - Start()
  - OnDisable()
  - OnDestroy()
  - FixedUpdate()
  - Update()
  - LateUpdate()
  - OnTriggerEnter()
  - OnTriggerStay()
  - OnTriggerExit()
  - OnCollisionEnter()
  - OnCollisionStay()
  - OnColissionExit()
  - Demais callbacks da Unity.
  - Métodos com atributos da Unity como RuntimeInitializeOnLoadMethod()
  - Outros métodos.


## Declare apenas uma variável por linha

Declarar apenas uma variável por linha é fundamental para o controle de versionamento, dessa forma quando você precisar editar ou adicionar uma variável, o git vai saber apontar melhor o que foi modificado, já que seu controle é feito através de linhas.

## Nunca divida seu código em Regions

Ter uma classe está grande o suficiente para que você considere dividi-la em regiões, é um forte sintoma de que ela está responsável por mais funções do que deveria, considere refatorar a classe e dividi-la em duas ou mais classes com menos responsabilidades.
	
## Sempre coloque auto-properties na mesma linha

Isso ajuda a facilmente diferenciar uma auto-propertie das demais properties.

```c#
public int MyProperty { get; set; }
```

## Considere o agrupamento de multiplos `for`, `foreach`, `using` e `while` statements

Considere agrupar as declarações do mesmo tipo quando elas compartilharem com exatidão a mesma lógica.
	
```c#
foreach (int x in Lines)
foreach (int y in Columns)
{
    using (var xReader = new StreamReader($"{x}.txt"))
    using (var yReader = new StreamReader($"{y}.txt"))
    {
        string xLine = xReader.ReadLine();
        string yLine = yReader.ReadLine();

        while (xLine != null && yLine != null) 
        {
            print($"{xLine} - {yLine}");

            xLine = xReader.ReadLine();
            yLine = yReader.ReadLine();
        }
    }
}
```


# Assets
## Coletânea de scripts Drag and Drop
Utilizado para a criação de objetos que utilizem da mecânica de clicar e arrastar objetos para posições específicas na tela, com objetivo de completar quebra-cabeças e afins. Compatível com GameObjects em geral, como scriptable objects,prefabs e prefabs variants. 
[Clique aqui](https://gist.github.com/ObjetosSenac/606b8a146d05cea9afc16542761ffbcf) para acessar a coletânea de scripts de Drag and Drop

### Drag Controller Script
[Clique aqui](https://gist.github.com/ObjetosSenac/606b8a146d05cea9afc16542761ffbcf#file-dragcontroller-cs)
• Crie um GameObject vazio em cena e anexe o script acima. Este será o script responsável por entender qual objeto foi clicado, seu atual estado (se foi encaixado ou ainda pode ser arrastado) e o que deve acontecer ao soltar o objeto.

### Draggable Script
[Clique aqui](https://gist.github.com/ObjetosSenac/606b8a146d05cea9afc16542761ffbcf#file-draggable-cs)
• Anexe este script ao objeto que você deseja clicar e arrastar.
• Para que seja possível clicar e arrastar, é necessário adicionar os componentes "RigidBody 2D" configurado como Kinematic e um "2D Collider" de sua preferência e que interaja com Raycasts.

### Slot Script
[Clique aqui](https://gist.github.com/ObjetosSenac/606b8a146d05cea9afc16542761ffbcf#file-slot-cs)

### Layer Script
[Clique aqui](https://gist.github.com/ObjetosSenac/606b8a146d05cea9afc16542761ffbcf#file-layer-cs)

### Configuração Drag And Drop

Teste
