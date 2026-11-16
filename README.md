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
| Risco geológico e inundação | risco.geojson | GeoSampa, IPT |
| Áreas contaminadas | contaminadas.geojson | CETESB |
| APP e cursos d’água | app.geojson | GeoSampa |
| Cobertura vegetal | vegetacao.geojson | MapBiomas, PMMA |
| Proteções | protecoes.geojson | CONPRESP, CONDEPHAAT, IPHAN, GeoSampa |

- `perimetros.geojson`: pontos dos perímetros com ficha (propriedades: id, nome, territorio, aprox, protecao). A posição da Chácara das Jaboticabeiras é **aproximada**; troque pela posição oficial. Esta versão só aceita Point.
- `juridico.json`: registros curados, ligados ao perímetro por `perimetro` (campos: data, instancia, registro, parte, status, fonte). Os dois registros de exemplo precisam ser conferidos nos atos oficiais.

## Pendências conhecidas
- Os centros dos territórios no código (const T) são aproximados; use o centroide do polígono de distritos.
- O mapa-base é o OpenStreetMap, com tiles públicos. A política de uso deles não serve para tráfego alto; em produção, use um provedor de tiles (MapTiler, Stadia, OpenFreeMap ou Protomaps) e troque o `style` em index.html.
- Indicadores por perímetro ("[a calcular]") dependem do cruzamento espacial das camadas, ainda não implementado.
- Zona norte e Centro não têm território no piloto.
