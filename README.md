import pytest
from task_2 import all_division


# проверяет деление
@pytest.mark.smoke
def test_simple_division():
    assert all_division(10, 2) == 5


# проверяет последовательное деление нескольких чисел
@pytest.mark.smoke
def test_multiple_divisions():
    assert all_division(100, 2, 5) == 10


# проверяет деление отрицательного числа
def test_negative_case():
    assert all_division(-20, 2) == -10


# проверяет деление числа с плавающей точкой
def test_float_case():
    assert all_division(5.0, 2) == 2.5


# проверяетналичие ошибки при делении на ноль ZeroDivisionError
def test_division_by_zero():
    with pytest.raises(ZeroDivisionError):
        all_division(10, 0)

