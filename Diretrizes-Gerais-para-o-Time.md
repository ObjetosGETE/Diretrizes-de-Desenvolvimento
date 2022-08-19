
<h1>Diretrizes Gerais para o Time</h1>

Este documento descreve orientações gerais para todos os envolvidos no projeto.

- [E-mail](#e-mail)
  - [Sempre informe as entregas pelo grupo de e-mail](#sempre-informe-as-entregas-pelo-grupo-de-e-mail)
- [Discord](#discord)
  - [Sempre use o grupo do projeto para discutir sobre o projeto](#sempre-use-o-grupo-do-projeto-para-discutir-sobre-o-projeto)
  - [Grupos específicos](#grupos-espcificos)
- [SharePoint](#sharepoint)
- [Printscreen WebGL] (#printscreen-webgl)
  - [Cuidados] (#cuidados)
  - [Como fazer] (#como-fazer)

---

# E-mail

## Sempre informe as entregas pelo grupo de e-mail
O endereço do nosso grupo de e-mail é: Objetos@senacrs.com.br. Sempre que finalizar uma etapa do projeto, informe a todos através desse e-mail.

A formalização por e-mail é uma etapa importante da nossa entrega, uma tarefa somente poderá ser considerar como concluída após essa etapa ter sido cumprida.

# Discord

## Sempre use o grupo do projeto para discutir sobre o projeto


Objetos-Gete é o nosso servidor oficial no discord. Observe atentamente a estrutura do servidor para realizar suas publicações no canal adequado, por exemplo, evite postar estudos sobre concept art no canal de arte 3D.

Informações específicas sobre projetos devem ficar no canal do projeto, dessa forma mantemos um histórico do nosso processo de desenvolvimento, que nos ajudará em projetos futuros.

## Grupos específicos

Dentro do servidor cada projeto possuiu um grupo de canais dedicados, a estrutura dos canais será sempre algo semelhante a lista abaixo:

* Docuentação
* Game Design
* Arte-3D
* Desenvolvimento
* UI/UX


Podendo ter mais ou menos canais de acordo com a especificidade de cada projeto.

# SharePoint

## Localização na rede

A pasta principal dos Objetos Gete no seguinte endereço:

https://senacrs365.sharepoint.com/sites/ObjetosGETE/Documentos%20Partilhados/Forms/AllItems.aspx?ga=1&viewid=cda4c95c-02b4-40f7-b7ed-f5e7257b79b5

Objetos educacionais desenvolvidos antes da formação do time de objetos ficam localizados na pasta da equipe de Sistemas no seguinte endereço:

https://senacrs365.sharepoint.com/:f:/s/Sistemas-GETE/EvY71wRPHL5NiZ--MjfXW1kBXOK0WoINTbYH6_tTVfy9Gg?e=Hkc0o8

## Centralização da produção

Durante o desenvolvimento de projetos usaremos diferentes ferramentas para compartilhamento de arquivos e para comunicação, e isso e totalmente aceitável visto que não queremos limitar nossa capacidade produtiva, entretanto, não podemos nos esquecer que o Sharepoint é nosso repositório oficial e portanto toda a produção deverá estar arquivada nele.

Um projeto que existe no Github, deverá ser upado no sharepoint após ser finalizado. Concepts de arte, modelos finalizados, ícones e demais produções artísticas também devem estar no Sharepoint.

Quando precisar compartilhar produções com o restante do time, de preferência por upa-lo no sharepoint e distribuir o link no discord.

# Printscreen WEBGL

## Cuidados
Durante o briefing e a definição do projeto, caso seja mencionado que é necessário salvar prints da tela, é de suma importância levantar as seguintes questões:
• Certificar-se de que o jogo será 2D. Há muita perda de performance em WebGL para permitir que a print seja feita;
• Caso o jogo seja 2D a build exceda ~60mb, questionar a real necessidade da mecânica pelas mesmas questões performáticas.

## Como fazer
Para realizar o print da tela, estaremos montando um script no index.html que utiliza a CDN html2canvas.min.js, chamando este método em uma .jslib e então registrando este método como uma DLL dentro da unity através de um .cs
  * Documentação da Unity de como chamar scripts de uma .jslib para dentro de um .cs: https://docs.unity3d.com/Manual/webgl-interactingwithbrowserscripting.html
  * CDN Html2Canvas: https://html2canvas.hertzen.com/
  * Após compilar a build, é necessário realizar uma alteração "no BuildLoader.js", sendo este um código minificado. Neste arquivo alteramos a variável "preserveDrawingBuffer:!1" para "preserveDrawingBuffer:1". Desta forma, desativamos a limpeza automática do buffer de rendering, permitindo que as imagens não saiam pretas no canvas. Por causa desta alterações que ocorrem os problemas de performance mencionados anteriormente.

Para exemplos, podemos utilizar o projeto "Crescendo com Saúde - Monte seu prato" para visualizar como é feita esta funcionalidade: https://github.com/ObjetosGETE/CCS-Monte-Seu-Prato | https://senacrs365.sharepoint.com/:u:/s/FS-GETE-MATERIAIS/EbiL0o_gFNFDvelBzv2o6VcBeH7IASHR2QTKIItYKkalWQ?e=gRWipZ


<h3>WIP - Este documento está em construção</h3>
