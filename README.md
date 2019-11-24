# Тестовое задание
Есть высоконагруженный веб-сервис, который должен отвечать веткой деревьев,
доставаемых из очень медленной БД, при этом сами данные — это CSV-файл с именем
дерева.csv, которое состоит из столбцов id, name, parent, где parent – это id родителя
элемента. Пусть таких файлов-деревьев три с именами tree1.csv, tree2.csv, tree3.csv.
Запрос на ветвь имеет вид:
{
tree: название файла с деревом,
id: id элемента, с которого строится ветвь
}
Требуется, чтобы сервис порождал заданное кол-во воркеров wMax для обработки входящих
HTTP-запросов, при первом обращении к сервису к какому-либо дереву:
1. проверял, есть ли это дерево в кэше, если есть и дата модификации файла данного
дерева не изменилась — отдавал ветку в виде многоуровнего JSON файла:
{

id: …,
name: ...,
children: [
{
id: …,
name: …,
children: null
},
...
]
}
2. если дата модификации дерева изменилась, либо кэша нет, то порождал воркер на
создание данного кэша и отдавал ветку, вручную собрав ее из файла (со структурой
как в п.1) и не дожидаясь окончания воркера перестройки кэша.

Учесть, что запросов может быть до 100 и более в секунду.

## Запуск и выполнение запроса

`yarn start`

Получить дерево:
`http://localhost:3000/?query={"tree":"tree3","id":1}`