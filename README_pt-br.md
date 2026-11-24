### O que é este fork?

É um gerador de imagens de mapa 100% automático, destinado à visualização em sites, "como o Google Maps".
Ele gera imagens `.png` de mapas de servidores OTS (cada imagem contém 8x8 tiles).
Também pode gerar imagens das casas para uso em sites.

### Como usar?

GERAR IMAGENS PNG A PARTIR DE UM OTBM
-----------------------
1. Compile o projeto como faria com um OTClient normal:

    https://github.com/edubart/otclient/wiki/Compiling-on-Windows

    https://github.com/edubart/otclient/wiki/Compiling-on-Linux

    https://github.com/edubart/otclient/wiki/Compiling-on-Mac-OS-X

    https://github.com/edubart/otclient/wiki/Compiling-for-Android

        Você também pode baixar o arquivo binário na página GitHub Releases.

2. Copie os arquivos **.spr e .dat** do cliente do seu servidor para a pasta do OTClient: **data/things/VERSAO_DO_PROTOCOLO/**

3. Copie o arquivo **data/items/items.otb** do servidor para a pasta do OTClient: **data/things/VERSAO_DO_PROTOCOLO/**

4. Copie o arquivo **data/world/NOME_DO_MAPA.otbm** do servidor para a pasta do OTClient: **data/map.otbm**

5. Execute o **otclient.exe**

6. Digite no terminal do cliente um comando semelhante a este:

   **prepareClient(1076, '/things/1076/items.otb', '/map.otbm', 8, 5)**

    - com a versão do protocolo do seu cliente;

    - caminhos válidos para items.otb e map.otbm;

    - número de threads a serem executadas (no exemplo, 8);

    - número de partes do mapa (o OTC dividirá o mapa em partes para reduzir o uso máximo de RAM).

    [Os caminhos são relativos. Comece com `/` para acessar a pasta `data`. O acesso a outras pastas do computador é bloqueado.]

7. Após 10 a 50 segundos, o OTClient mostrará algo semelhante a isto (o cliente ficará travado durante esse processo; não o feche, apenas aguarde):

    Agora digite (níveis inferiores com 30% de sombra)
    TODAS AS PARTES DO MAPA:
    generateMap('all', 30)
    SOMENTE AS PARTES 2 E 3 DO MAPA:
    generateMap({2, 3}, 30)

8. Digite no terminal do cliente:

    **generateMap('all', 30)**

    O OTClient exibirá o progresso no terminal.

9. As imagens do mapa aparecerão no diretório do OTClient:

    **exported_images/map** - abra esta pasta no explorador de arquivos.

10. Copie o diretório **map**, localizado em **exported_images**, para o diretório **website_and_php_files** do otclient_mapgen.

    --- OS PRÓXIMOS PASSOS EXIGEM QUE O **PHP** ESTEJA INSTALADO NO SISTEMA ---

    --- LINUX: DURANTE A GERAÇÃO, CONCEDA PERMISSÃO 777 À PASTA **website_and_php_files** ---

GERAR OS NÍVEIS DE ZOOM DO MAPA PARA O SITE
-----------------------
### Linux

1. Execute no terminal:

    **sh linux_run_all.sh**

### Windows

##### 1. Se você tiver o Git Bash instalado

1. Execute no terminal do sistema:

    **./windows_run_1_pre_tile_generator.sh**

2. Execute:

    **./windows_run_2_tile_generator.sh**

    Observe o uso da CPU pelo php.exe e aguarde a conclusão.

3. Execute:

    **./windows_run_3_pre_compress.sh**

4. Execute:

    **./windows_run_4_compress.sh**

    Observe o uso da CPU pelo php.exe e aguarde a conclusão.

##### 2. Se você não tiver o Git Bash

1. Execute no terminal do sistema:

    **php 1_pre_tile_generator.php**

2. Execute com o parâmetro que representa o "andar" do mapa:
    - Os comandos podem ser executados em qualquer ordem.
    - É possível abrir vários terminais e executar vários comandos ao mesmo tempo para gerar o mapa mais rapidamente (usando todos os núcleos da CPU).

    **php 2_tile_generator.php 0**

    **php 2_tile_generator.php 1**

    **php 2_tile_generator.php 2**

    **php 2_tile_generator.php 3**

    **php 2_tile_generator.php 4**

    **php 2_tile_generator.php 5**

    **php 2_tile_generator.php 6**

    **php 2_tile_generator.php 7**

    **php 2_tile_generator.php 8**

    **php 2_tile_generator.php 9**

    **php 2_tile_generator.php 10**

    **php 2_tile_generator.php 11**

    **php 2_tile_generator.php 12**

    **php 2_tile_generator.php 13**

    **php 2_tile_generator.php 14**

    **php 2_tile_generator.php 15**

    **php 2_tile_generator.php 16**

3. Execute:

    **php 3_pre_compress.php**

4. Execute com o parâmetro que representa o "andar" do mapa:
    - A qualidade da compressão pode ser definida no arquivo 4_compress.php (linha: `$quality = 80;`)
    - Os comandos podem ser executados em qualquer ordem.
    - É possível abrir vários terminais e executar vários comandos ao mesmo tempo para gerar o mapa mais rapidamente (usando todos os núcleos da CPU).

    **php 4_compress.php 0**

    **php 4_compress.php 1**

    **php 4_compress.php 2**

    **php 4_compress.php 3**

    **php 4_compress.php 4**

    **php 4_compress.php 5**

    **php 4_compress.php 6**

    **php 4_compress.php 7**

    **php 4_compress.php 8**

    **php 4_compress.php 9**

    **php 4_compress.php 10**

    **php 4_compress.php 11**

    **php 4_compress.php 12**

    **php 4_compress.php 13**

    **php 4_compress.php 14**

    **php 4_compress.php 15**

    **php 4_compress.php 16**

## Configurar o site
15. Mova a pasta **website_and_php_files/map_viewer** para o servidor web. Ela contém todas as imagens e os scripts necessários para visualizá-las (Leaflet Map).

16. Configure o map_viewer. A configuração fica no início do arquivo **map.js**:
- As imagens podem permanecer em outro servidor web. Defina **imagesUrl** com a URL do servidor, começando com `http` (ou `https`): `http://meuservidor.com/map_images/`
```
    imagesUrl: 'map/', // URL da pasta com os níveis de zoom (pastas de 0 a 16)
    imagesExtension: '.jpg',
    mapName: 'RL MAP?',
    startPosition: {x: 32000, y: 32000, z: 7},
    startZoom: 14,
    minZoom: 4,
    maxZoom: 18, // o máximo com qualidade completa é 16
```

GERAR IMAGENS DAS CASAS
-----------------------

1. Execute os passos 1 a 6 das instruções **GERAR IMAGENS PNG A PARTIR DE UM OTBM**, mostradas acima.

2. Digite no terminal do cliente:

   **generateHouses(30, false)**

    - **30** - percentual de sombra para os tiles ao redor da casa (eles ficarão mais escuros);

    - **false** - carrega o mapa inteiro de uma vez e exige muita RAM. Defina como **true** para carregar o mapa por partes.
    Esse modo é mais lento, mas usa menos RAM para gerar as imagens das casas.

    **O cliente ficará travado durante a geração das imagens das casas!** Não o feche.
    Para acompanhar o progresso, execute o OTClient pelo terminal do Windows ou Linux (não clicando no `.exe`):

    ./otclient

    O cliente ficará travado, mas mostrará no terminal do sistema as mensagens sobre as casas geradas.

3. As imagens das casas aparecerão no diretório do OTClient:

    **exported_images/house/**

GERAR O MINIMAPA COMPLETO PARA O OTCLIENT
-----------------------

1. Execute os passos 1 a 6 das instruções **GERAR IMAGENS PNG A PARTIR DE UM OTBM**, mostradas acima.

2. Digite no terminal do cliente:

   **saveMinimap('minimap.otmm')**

3. O minimapa aparecerá no diretório do usuário do sistema:

    **%HOMEPATH%/otclient/** - Windows; abra esta pasta no explorador de arquivos

    **${HOME}/.otclient/** - Linux; use `cd` para acessá-la. Esta pasta é oculta, mas pode ser acessada normalmente.

4. Copie o arquivo para a pasta **data** do seu OTClient normal.
O OTClient precisa estar fechado ao substituir o minimapa por esta nova versão.

5. Execute o OTClient, faça login no servidor e o minimapa completo deverá estar disponível.
