1Е Задание
import random
import string

def generate_random_name():
while True: # генерируем случайные имена
len1 = random.randint(1, 15) # Первое имя ( длинной от 1 до 15ти символов)
len2 = random.randint(1, 15) # Второе имя ( длинной от 1 до 15ти символов)
word1 = ''.join(random.choice(string.ascii_lowercase) for _ in range(len1)) # получаем строку с всеми буквами алфавита, выбираем из нее букву (несколько раз), соединяем в имя
word2 = ''.join(random.choice(string.ascii_lowercase) for _ in range(len2)) # то же что и в строке выше но для второго имени
yield f"{word1} {word2}" # возвращаем рандомное имя ( состоящее из 2х слов)

2Е Задание
import pytest # импортируем библиотеку 'pytest'

def all_division(*arg1): # берем первый аргумент, принимаем как начальный и бробим на следующие
division = arg1[0]
for i in arg1[1:]:
division /= i
return division

def test_positive_numbers(): # Проверяем правильность деления
assert all_division(100, 10, 2) == 5

@pytest.mark.smoke
def test_smoke_positive(): # смок тест с положительными числами
assert all_division(20, 2, 5) == 2

def test_negative_numbers(): # Проверяем правильность деления на отрицательные числа
assert all_division(100, -10, 2) == -5

@pytest.mark.smoke # смок тест с отрицательными числами
def test_smoke_negative():
assert all_division(-20, 2, 5) == -2

def test_division_by_zero(): # проверка деления на ноль
with pytest.raises(ZeroDivisionError):
all_division(10, 0)

3Е Задание
import pytest

def all_division(*arg1):
division = arg1[0]
for i in arg1[1:]:
division /= i
return division

@pytest.mark.parametrize( # Принимаем два аргумента, строку сименами параметрову и список кортежей
"a, b, expected",
[
(100, 10, 1, 1.0),
pytest.param(20, 2, 5, 2, marks=pytest.mark.smoke), #Помечаем набор данных как смок тест ( с положительными числами)
(100, -10, 2, -5.0),
pytest.param(-20, 2, 5, -2, marks=pytest.mark.smoke), #Помечаем набор данных как смок тест (с отрицательными числами)
pytest.param(10, 2, 0, id="skip_because_zero",
marks=pytest.mark.skip(reason="Деление на ноль низя")), # скипаем набор данных с обьяснением
],
)
def test_all_division_parametrized(a, b, c, expected): # запускаем несколько раз с параметрами из '@pytest.mark.parametrize'
assert all_division(a, b, c) == expected

4Е задание
import pytest
import time

class TestExample:
@pytest.fixture(autouse=True, scope="class")
def setup_class(self, request):
# Выполняется автоматически перед всеми тестами класса
start_time = time.time() # Начало времени выполнения
print(f"\n[INFO] Начало тестов класса {request.node.name} в {time.strftime('%H:%M:%S')}")
yield
end_time = time.time() # Окончание времени выполнения
print(f"\n[INFO] Завершение тестов класса {request.node.name} в {time.strftime('%H:%M:%S')}") # заворачиваем время в нужный формат
print(f"[INFO] Общая продолжительность выполнения: {end_time - start_time:.2f} секунд")

@pytest.mark.usefixtures("track_test_time")      # Определяем 3 отдельных теста
def test_example_1(self):
    time.sleep(1)
    assert 1 + 1 == 2

def test_example_2(self):
    time.sleep(2)
    assert 2 * 2 == 4

@pytest.mark.usefixtures("track_test_time")
def test_example_3(self):
    time.sleep(0.5)
    assert 3 - 1 == 2
