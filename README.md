# hands_test
Тестовое задание hands.ru

За основу берём https://github.com/andrewisakov/xt_test (реализация зависимых скидок и структура базы).
Правильнее, конечно, строже относиться к заполнению того самомго поля «телефон» в форме:). Хотя бы маску добавить. Ну, это так, философия... :)

Получаем этот самый «user input». Учитывая, что номер могут ввести любым странным способм, пробелы, дефисы и скобки стнавятся ничтожными.

Вычистим все буквенные символы. Хотя бы так:
re.sub(r"[^0-9+._\s-]+", "", user_input).replace(' ','').
Получим строку с расстояниями между цифрами в виде символов «_».

Окончательная команда (развитие предыдущей)
may_be_phones = [p.replace('_','') for p in re.sub(r"[^0-9+_\s-]+", "_", strs).replace(' ','').split('__') if p]
Сплитим строку по двойному «_», удялем из полученных элементов «_» попутно избавляемся от пустых строк.

Получим список числовых строк, расстояние между которыми более одного нецифрового символа (и даже слова). Пробелы в номере, как и дефисы, полагаем допустимыми.

Теперь их нужно обработать на предмет соответствия федеральным и прямым (городским номерам). Городские могут быть 6 и 7 значными. Федеральные - 10-12 (+7|8)?.

Проверка на принадлежность элемента списка к тому или иному пулу номеров реализовано в https://github.com/andrewisakov/taximaster_x/blob/master/distributor/database.py
в def select_distributor (рабочий код).
База получена из таблиц Россвязи (с сайта).

Проверку на дупликаты заказов можно выполнить, собрав все заказы за какой-то обозреваемый период из модели Order (см начало), переведя item_id из заказов в set() и выполнив операции дизъюнкции между наборами из разных заказов (если они были в обозреваемый промежуток времени).

Задания можно отправлять не rabbitmq посредство celery, обёрнутого в multiprocessing пул. Ибо celery - блокирующий.

Всё.
Такая я ленивая скотина... :) В смысле отсылки к другим моим репам...
