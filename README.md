# SITE HIS — piloto (MapLibre)

## Rodar
    cd site-his-maplibre
    python3 -m http.server 8000
Abra http://localhost:8000. Abrir o index.html direto no navegador não funciona: o navegador bloqueia a leitura dos arquivos em data/.

## Dados (pasta data/)
Cada camada é um GeoJSON (WGS84, EPSG:4326). Se o arquivo não existe, a camada aparece como "sem arquivo" no painel. Converta shapefiles com `ogr2ogr -f GeoJSON -t_srs EPSG:4326 saida.geojson entrada.shp`.

| Camada | Arquivo | Fonte provável |
|---|---|---|
| Distritos | distritos.geojson | GeoSampa / Prefeitura |
| HIS e ZEIS | his.geojson | GeoSampa, SEHAB, CDHU |
| HIS possíveis (**DEMO, fictício**) | his_mock.geojson | Gerado para demonstração |
| Risco geológico e inundação | risco.geojson | GeoSampa, IPT |
| Áreas contaminadas | contaminadas.geojson | CETESB |
| APP e cursos d’água | app.geojson | GeoSampa |
| Cobertura vegetal | vegetacao.geojson | MapBiomas, PMMA |
| Proteções | protecoes.geojson | CONPRESP, CONDEPHAAT, IPHAN, GeoSampa |

- `perimetros.geojson`: pontos dos perímetros com ficha (propriedades: id, nome, territorio, aprox, protecao). A posição da Chácara das Jaboticabeiras é **aproximada**; troque pela posição oficial. Esta versão só aceita Point.
- `juridico.json`: registros curados, ligados ao perímetro por `perimetro` (campos: data, instancia, registro, parte, status, fonte). Os dois registros de exemplo precisam ser conferidos nos atos oficiais.

## Dados de demonstração (his_mock.geojson)
- 14 polígonos **inventados** nos 5 territórios. Nada ali é empreendimento real.
- Campos: id, nome, territorio, tipo (ZEIS 1/2/3, Empreendimento HIS 1/2, etc.), fase (proposta, em aprovação, em obras, entregue), unidades, agente, nota, demo.
- A cor do polígono segue `fase`. O clique abre um popup marcado "Dado fictício".
- Na ficha da Chácara das Jaboticabeiras, a aba Visão geral conta as HIS a até 500 m do pino (círculo tracejado no mapa). Das 3 de Vila Mariana, 2 ficam dentro (220 m e 398 m) e 1 fora (947 m). É a prévia do cruzamento espacial que hoje aparece como "[a calcular]".
- Para tirar a demo: apague a linha `hism` do array `L` em index.html, o `<div id="demo">` e o arquivo. Para usar dados reais, mantenha o `id` da camada e os mesmos campos.

## Pendências conhecidas
- Os centros dos territórios no código (const T) são aproximados; use o centroide do polígono de distritos.
- O mapa-base é o OpenStreetMap, com tiles públicos. A política de uso deles não serve para tráfego alto; em produção, use um provedor de tiles (MapTiler, Stadia, OpenFreeMap ou Protomaps) e troque o `style` em index.html.
- Indicadores por perímetro ("[a calcular]") dependem do cruzamento espacial das camadas, ainda não implementado.
- Zona norte e Centro não têm território no piloto.
