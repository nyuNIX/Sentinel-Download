# Exportação de imagens Sentinel-2 com filtragem por nuvens (Google Colab + Earth Engine)
Este projeto foi desenvolvido para ser executado no Google Colab, utilizando a API do Google Earth Engine (GEE) para automatizar a seleção e exportação de imagens Sentinel-2 livres (ou quase livres) de nuvens sobre polígonos de interesse (talhões). O código realiza a montagem do Google Drive, a autenticação interativa do Earth Engine e o processamento completo das imagens, exportando os resultados diretamente para pastas organizadas no Drive. Para executar, é necessário que o usuário possua uma conta com acesso ao Earth Engine e que o projeto do GEE esteja previamente habilitado.

A autenticação ocorre de forma interativa: ao rodar o notebook, o usuário deve autorizar o acesso ao Google Drive e seguir o link fornecido pelo ee.Authenticate() para gerar e colar o token de autenticação do Earth Engine. Os polígonos de entrada podem estar em formato GeoJSON, Shapefile (.shp) ou Shapefile compactado (.zip), armazenados no Google Drive. Para cada arquivo de polígonos, o código cria automaticamente uma subpasta de saída, mantendo uma organização clara dos dados exportados.

O processamento associa cada imagem Sentinel-2 SR à sua respectiva imagem de probabilidade de nuvem (S2Cloudless) por meio de um join exato baseado no system:index. A filtragem de nuvens é feita avaliando a fração de pixels classificados como nuvem dentro do talhão, permitindo tolerância controlada. Imagens adquiridas na mesma data, mas pertencentes a diferentes tiles MGRS, são mantidas, pois podem apresentar condições distintas de cobertura de nuvens sobre a área de interesse, aumentando a robustez do conjunto de dados.

Um ponto em aberto neste projeto está relacionado à definição dos parâmetros
`cloudProbThreshold` (limite de probabilidade de nuvem por pixel) e
`maxAllowedCloudPercentage` (fração máxima de nuvem permitida no talhão),
considerando que o objetivo final é treinar um modelo de IA para identificar
o tipo de cultivo e seu estágio de maturação. Algumas questões ainda precisam
ser respondidas para orientar essa escolha:

- O modelo final será baseado em imagens individuais (ex.: CNN) ou em séries
  temporais completas (ex.: SITS-Former)?
- Qual é a tolerância real do modelo escolhido à presença de nuvens parciais
  ou ruído atmosférico?
- É mais vantajoso descartar agressivamente imagens com nuvem ou manter maior
  cobertura temporal e permitir que o modelo aprenda a lidar com imperfeições?
