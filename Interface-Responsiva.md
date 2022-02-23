<h1>Como manter uma interface responsiva em projeto WebGL feitos em Unity</h1>

Responsividade é fundamental para a que o usuário tenha a melhor experiência possível, existem diversas técnicas diferentes para se construir UI dentro da Unity, neste documento vamos descrever os processos para elaboração de uma interface que seja responsiva para um ambiente web e mobile e ao mesmo tempo fácil para o desenvolvedor implementar dentro da ferramenta.

Nosso [Template Unity](https://github.com/SistemasGETE/UnityTemplate) já está configurado para manter uma boa responsividade em navegadores e dispositivos móveis, ele é um excelente ponto de partida e uma referência de implementação para tudo o que está descrito neste documento.

- [Main Canvas](#main-canvas)
  - [Configurando o Canvas](#configurando-o-Canvas)
  - [Canvas Scaler](#canvas-scaler)
  - [Graphic Raycaster](#graphic-raycaster)
  - [Auto Assign Main Camera To Canvas](#auto-assign-main-camera-to-canvas)
  
- [Aspect Ratio Fitter](#aspect-ratio-fitter)
  - [Configurando o Aspect Ratio Fitter](#configurando-o-aspect-ratio-fitter)
  - [Posição Absoluta](#posição-absoluta)
  
- [Main Camera](#main-camera)
  - [Configurando a Main Camera](#configurando-a-main-camera)
  - [Posição Absoluta](#posiÃ§Ã£o-absoluta)

---

# Main Canvas

## Configurando o Canvas

O Canvas deve estar na raiz da cena, sendo o primeiro elemento da hierarquia. E Deve ser configurado da seguinte forma:

![Image](/Imagens/Interface%20Responsiva/canvas.png)
 

`Render Mode` Configurar em "Screen Space – Camera" assim permite-se que o canvas se ajuste em tempo de execução ao aspect ratio da câmera, além de ser essencial para os nossos ajustes de acessibilidade. Todos os nosso projetos precisam dar suporte para diferentes tipos de daltonismo e a única forma de aplicarmos diferentes configurações de cores ao canvas é se ele for renderizado no mundo, em frente a câmera.

`Render Camera` – Linkar aqui a Main Camera da cena.

`Plane Distance` – 0.4 é considerada uma distância ideal, esse valor pode ser ajustado caso o canvas não esteja sendo exibido pela câmera.

As demais configurações são dependes do projeto e não serão discutidas nesse documento.
## Canvas Scaler
![Image](/Imagens/Interface%20Responsiva/scaler.png)

Adicionar o `Canvas Scaler` ao mesmo objeto que contém o canvas, 1920x1080 é a nossa resolução padrão, é possível que algum de nossos projetos tenha outra resolução de referência, nesse caso o `Canvas Scaler` deve ser alterado para refletir a mudança.
Definir o modo de escala como Screen Size e deixar o Match sempre em acordo com a largura.
## Graphic Raycaster

![Image](/Imagens/Interface%20Responsiva/graycaster.png)
 
O `Graphic Raycaster` é um elemento necessário para que o canvas responda às interações do usuário.

## Auto Assign Main Camera To Canvas.
![Image](/Imagens/Interface%20Responsiva/auto.png)
 
Um dos nossos scripts utilitários, deve ser adicionado ao Canvas para que ele linke automaticamente a câmera principal, caso ela não tenha sido assinalada no inspector.

```csharp
using UnityEngine;

/// <summary>
/// Add this to the Main Canvas to auto assign a camera to it.
/// </summary>
[RequireComponent(typeof(Canvas))]
public class AutoAssignMainCameraToCanvas : MonoBehaviour
{
    private Canvas _myCanvas;

    private void Start()
    {
        _myCanvas = GetComponent<Canvas>();

        if (_myCanvas.worldCamera != null)
            return;

        else if (!Camera.main)
        {
            Debug.LogError("This script requires a Camera with 'MainCamera' tag to be present on the scene");
            return;
        }

        _myCanvas.worldCamera = Camera.main;
    }
}
```

O elemento de canvas completo, fica da seguinte forma:

![Image](/Imagens/Interface%20Responsiva/maincanvas.png)

# Aspect Ratio Fitter

## Configurando o Aspect Ratio Fitter
![Image](/Imagens/Interface%20Responsiva/fitter.png)

O Aspect Ration Fitter deve ser filho do Canvas e pai de todos os outros elementos da interface, ele que irá comprimir ou expandir seus elementos filhos para que eles encaixem no Aspect Ratio.

Como definimos nossa resolução padrão como 1920x1080 o valor Aspect Ratio deve ser 16/9, que é aproximadamente 1,777778

## Posição Absoluta

![Image](/Imagens/Interface%20Responsiva/absoluta.png)

Colocar elementos na interface com posição absoluta é considerado uma péssima prática dentro da Unity, e as técnicas mais comuns para se trabalhar com UI, são as posições relativas através da ancoragem dos elementos em relação a sua distância do canto da tela.

Porém, com nossa configuração conseguimos, sem medo, utilizar os elementos da UI em posição absoluta, desde que todos os elementos sejam filhos do Aspect Ratio Fitter

# Main Camera

## Configurando a Main Camera
![Image](/Imagens/Interface%20Responsiva/maincamera.png)

É importante que a Tag da Main Camera esteja definida como "MainCamera". É necessário adicionar o script "Keep Aspect Ratio" e preencher o campo "Target Aspect Ratio" com o mesmo valor inserido previamente no Aspect Ratio Fitter, que no nosso caso é 16/9 ou aproximadamente 1,777778.

## Keep Aspect Ratio

Script responsável por manter a resolução do usuário em um valor de aspect ratio pré definido (16/9), mesmo que a janela do browser mude dinamicamente de tamanho por ações do usuário. Esse script deve ser adicionado à Camera.
```csharp
using UnityEngine;

public class KeepAspectRatio : MonoBehaviour
{
    //Por padrão vamos manter o aspect ratio em 16:9, caso algum projeto específico precise
    // de outro aspect ratio, lembrar também de alterar nos elementos do canvas.
    [SerializeField] private float _targetAspectRatio = 16f / 9.0f;
    private Camera _myCamera;
    private Rect _cameraRect;

    private void Awake()
    {
        _myCamera = GetComponent<Camera>();
        _cameraRect = _myCamera.rect;
    }

    private void Update()
    {
        // determine the game window's current aspect ratio
        float windowaspect = (float)Screen.width / (float)Screen.height;

        // current viewport height should be scaled by this amount
        float scaleheight = windowaspect / _targetAspectRatio;

        // if scaled height is less than current height, add letterbox
        if (scaleheight < 1.0f)
        {
            _cameraRect.width = 1.0f;
            _cameraRect.height = scaleheight;
            _cameraRect.x = 0;
            _cameraRect.y = (1.0f - scaleheight) / 2.0f;

            _myCamera.rect = _cameraRect;
        }
        else // add pillarbox
        {
            float scalewidth = 1.0f / scaleheight;
            
            _cameraRect.width = scalewidth;
            _cameraRect.height = 1.0f;
            _cameraRect.x = (1.0f - scalewidth) / 2.0f;
            _cameraRect.y = 0;

            _myCamera.rect = _cameraRect;
        }
    }
}
```

# Conclusão

Ao final, teremos uma interface 100% responsiva tanto para navegadores desktop quanto para navegadores mobile.

![Image](/Imagens/Interface%20Responsiva/responsividade.gif)