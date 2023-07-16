# Работа с заявками клиентов

## Договоры на абонентское обслуживание

Необходимо добавить новый вид договоров - абоненское обслуживание. В договорах такого вида пользователь должен иметь возможность внести дату начала действия договора, дату окончания действия договора, сумму ежемесячной абоненсткой платы и стоимость часа работы специалиста.

Для решения задачи:
1. Добавим новый элемент перечисления ВидыДоговоровКонтрагентов
2. Добавим в справочник ДоговорыКонтрагентов реквизиты для хранения периода действия договора, суммы абонентской платы и стоимость часа работы
3. Выведем на форму новые реквизиты договора, если выбран соответствующий вид договора.

*Помним, что изменения формы реализуем программно, новые объекты и методы добавляем с префиксом.*

## Обслуживание клиентов

Необходимо планировать обслуживание сотрудниками оборудование и программ клиентов. Процесс работы с заявками построен следующим образом:
1. Клиент звонит менеджеру и оставляет заявку на работу специалиста с указанием проблемы, для решения которой нужен специалист. Менеджер ищет свободное время и планирует заявку на это время.
2. Специалист в назначенное время в офисе клиента или удаленно проводит необходимые работы.
3. Специалист получает подписанный лист учета рабочего времени или скан/фото листа учета от клиента. В листе учета перечисляются виды работ, выполненные специалистом и фиксируется количество часов к оплате. В дальнейшем документ будет являться подтверждением проведения работ.
4. Специалист вносит в информационную систему информацию о выполненных работах, количество фактически потраченных часов на каждый вид работы, количество часов к оплате клиенту за каждый вид работы и прикрепляет к документу скан/фото листа учета рабочего времени.

Все объекты, связанные с обслуживанием клиентов должны располагаться в новой подсистеме Обслуживание клиентов.

Для решения задачи:
1. Добавим документ ОбслуживаниеКлиента
2. В документ добавим реквизиты:
    - Клиент
    - Договор
    - Специалист
    - Дата проведения работ
    - Время начала работ (план)
    - Время окончания работ (план)
    - Описание проблемы
    - Комментарий
3. В документ добавим табличную часть ВыполненныеРаботы с колонками:
    - Описание работ
    - Фактически потрачено часов
    - Часов к оплате клиенту
4. Реализуем форму документа и форму списка с разумным размещением полей, подключим к формам подсистему Подключаемые команды
5. Добавим возможность хранить присоединенные к документу файлы
6. Создадим роль для работы с документом
7. Добавим регистр накопления для хранения информации о суммах задолженности клиента ВыполненныеКлиентуРаботы
    - Измерения - Клиент, Договор
    - Ресурсы - Количество часов, Сумма к оплате
8. Реализуем проверку при проведении документа, что выбрать договор с типом абонентская плата и что дата документа лежи между датой начала и датой окончания действия договора
9. Реализуем обработку проведения по регистру накопления, сумма к оплате должна рассчитываться исходя из ставки часа, указанной в договоре
10. Добавим документ в подсистему Обслуживание клиентов

### Дополнительное задание для Обслуживания клиентов*
*Это задание не является обязательным требованием, однако вы можете изучить дополнительный материал, реализовать механизм и получить комментарий по реализованному механизму от дипломного руководителя.*

Для удобного планирования времени специалистов необходимо реализовать календарь загрузки на базе Планировщика.

Для решения задачи:
1. Изучим информацию о планировщике в справке о платформе 1С:Предприятие и материалам в открытом доступе
2. Добавим обработку Календарь специалистов, которая будет отображать документы Обслуживание клиентов в календаре в разрезе специалистов
3. Реализуем возможность создания обслуживания из календаря с автоматическим заполнением специалиста, времени начала и времени окончания
4. Реализуем автоматическое обновление данных планировщика при изменении документов Обслуживание клиентов с помощью метода Оповестить
5. Добавим календарь на начальную страницу
[Пример использования планировщика на канале сообщества 1С-Разработчиков Фирмы 1С](https://youtu.be/oHkJO9h7m9A?t=2893)

## Заполнение Реализации товаров и услуг

Если в документе Реализации товаров и услуг выбран договор с видом абонентская плата, то необходимо реализовать возможность автозаполнения такого документа суммой ежемесячной абонентской платы и суммой за выполненные в течения месяца работы по данным документов Обслуживание клиентов.
Из документа должен печататься акт об оказанных услугах.

Для решения задачи:
1. Добавим константы НоменклатураАбонентскаяПлата и НоменклатураРаботыСпециалиста с типом ссылка на справочник Номенклатура
2. Добавим на форму Реализации товаров и услуг команду Заполнить
3. Команда Заполнить должна проверять вид договора, если это договор абонентского обслуживания, то вызывать процедуру ВыполнитьАвтозаполнение из модуля объекта документа
3. В модуле объекта необходимо реализовать процедуру ВыполнитьАвтозаполнение, которая:
    - Получит номенклатуру из констант НоменклатураАбонентскаяПлата и НоменклатураРаботыСпециалиста, если хотя бы одна незаполнена необходимо выдать ошибку и прекратить выполнение процедуры.
    - Очистит табличную часть
    - Если в договоре не нулевая сумма абонентской платы, то добавит в табличную часть строку с номенклатурой из константы НоменклатураАбонентскаяПлата и суммой абонентской платы из договора
    - Если в месяц даты документа в регистре ВыполненныеКлиентуРаботы есть информация о выполненных работах по этому договору, то добавит в табличную часть строку с номенклатурой из константы НоменклатураРаботыСпециалиста и общим количеством и суммой из регистра ВыполненныеКлиентуРаботы за месяц документа
4. Реализуем для документа печатную форму Акт об оказанных услугах. Акт может быть произвольной формы, но должен содержать следующие данные:
    - Номер и дата документа
    - Информацию о Контрагенте, Организации и договоре
    - Детализацию из табличной части документа (Номенклатуру, Количество, Цену, Сумму)
    - Итоговую сумму цифрами и прописью
    - Поля для подписей ответственных лиц и печати
Акт может быть реализован в виде mxl или docx. У пользователя должна быть возможность редактирования макета акта.

## Массовое создание документов Реализация товаров и услуг

В начале месяца бухгалтер фирмы формирует акты по всем абонентским договорам. Необходимо автоматизировать эту процедуру.

Для решения задачи:
1. Создадим обработку МассовоеСозданиеАктов
2. Добавим реквизит обработки Период для указания месяца
3. Добавим табличную часть для хранения списка договоров и ссылок на созданные Реализации по этим договорам за период
4. Реализуем интерфейс для работы с обработкой
5. Реализуем алгоритм заполнения табличной части и создания реализаций со следующими особенностями:
    - Алгоритм должен выполняться с использованием механизма длительных операций БСП
    - Если Реализация за выбранный месяц уже создана, то обработка не должна создавать вторую
    - При создании Реализации необходимо вызывать стандартный алгоритм заполнения
    - Для заполнения Реализации необходимо использовать метод модуля объекта Реализации ВыполнитьАвтозаполнение
    - Перед проведением Реализации необходимо вызывать стандартный алгоритм проверки заполнения
6. Добавим обработку в подсистему Обслуживание клиентов
    
## Права доступа

Документы Обслуживание клиентов вводят специалисты и менджеры. Массовое создание документов Реализации использует бухгалтер.

Для решения задачи:
1. Создадим необходимые роли для работы с объектами
2. Добавим поставляемые профили групп доступа Специалист, Менеджер, БухгалтерИТФирмы
3. Добавим в профили роли