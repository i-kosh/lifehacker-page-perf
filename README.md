[Отчет с панели Performance](../main/assets/Profile-20210604T133657.json)  
[Отчет с панели Network (HAR)](../main/assets/lifehacker-net-perf.har)

_Использованная версия Google Chrome v93 dev_

# ШРИ 2021 ДЗ по теме "Использование браузерных DevTools - анализ сайта"

## Загрузка страницы и вкладка Network

### Найденные неоптимальные места:

1. Большинство изображенией сервится в неоптимальных форматах. К примеру самым большим ресурсом на транице является _PNG_ картинка:
   ![](../main/assets/img/chrome_PcigqrY2Ea.png)
   
   которую к тому-же даже не видно в изначальной позиции окна.
   
   ![](../main/assets/img/chrome_Rqrc7nxWlR.png)

   Этого можно было избежать добавив _lazyloading_ для большинства изображений и отдавать их в более эффективных форматах _webp_ и возможно _AVIF_ если его поддерживает браузер.

   Учитывая что большую часть контента страницы составляют изображения таким образом можно было бы добиться значительного сокращения размеров страницы

1. Некоторые запросы используют уставерший HTTP/1.1 протокол
   ![](../main/assets/img/chrome_dImGMeDO2s.png)

1. Болшинство запросов к текстовым ресурсам _(js, css)_ используют менее эффективный метод сжатия _gzip_,
   вместо него прелагается использовать более современный алгоритм _Brotli_, это поможет еще больше уменьшит размеры загружаемых ресурсов и сократить задержки сети

1. Некоторые ресурсы запрашиваются дважды без видимой необходимости
   ![](../main/assets/img/chrome_SbBDKYCLy4.png)
 
1. Уболшинства статических ресурсов запросы довольно долго обрабатываются сервером, что можно оптимизировать за счет кеширования на строне сервера.
   ![](../main/assets/img/chrome_bwkUhaudRl.png)
   
 ____

## Вкладка Perfomance

Тайминги:
|                        Название метрики |   FP  | LCP (FMP) |   DCL  |    L   |
|----------------------------------------:|:-----:|:---------:|:------:|:------:|
| Время прошедшее с начала навигации (ms) | 358.3 |   834.2   | 6114.2 | 6373.7 |

Метрики разных этапов обработки:
|             Название метрики | Loading | Scripting | Rendering | Painting |
|-----------------------------:|:-------:|:---------:|:---------:|:--------:|
| Общее затраченное время (ms) |   159   |    6723   |    485    |    96    |

## Вкладка Coverage

 Использование ресурсов:

![](../main/assets/img/chrome_lx9baLgkNz.png)

|                 Тип ресурса |  css |   js  |
|----------------------------:|:----:|:-----:|
| Неиспользованный объем (kB) | 75.1 | ~2458 |
