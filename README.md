# Luhn_Algorithm
_Алгоритм Луна (Luhn algorithm) или формула Луна_ - алгоритм вычисления контрольной цифры, получивший широкую популярность. Он используется, в частности, при первичной проверке номеров банковских пластиковых карт, номеров социального страхования в США и Канаде. Алгоритм был разработан сотрудником компании «IBM» Хансом Петером Луном и запатентован (патент США № 2 950 048) в 1960 году.

В настоящее время алгоритм находится в общественном достоянии. Контрольные цифры вообще и алгоритм Луна в частности предназначены для защиты от случайных ошибок, а не преднамеренных искажений данных.

#### Описание алгоритма
Алгоритм Луна позволяет выявить все одиночные ошибки и практически все перестановки соседних чисел в числовых последовательностях любой длины (не выявляются только перестановки нуля и девятки: «09» вместо «90» и наоборот). Более сложные алгоритмы позволяют выявить большее количество ошибок.

Так как алгоритм оперирует цифрами в обратном порядке (справа налево), нули в начале числа никак не влияют на значение контрольной цифры. Таким образом, системы, дополняющие проверяемое число нулями слева до определенной длины (например, «1234» до «0000001234»), могут проводить проверку как до дополнения, так и после него.

Алгоритм Луна работает только с последовательностями десятичных цифр.

#### Порядок вычисления контрольной суммы

Считая справа налево, удваиваем каждую вторую цифру последовательности.

Суммируем все одиночные цифры (т. е. если на предыдущем шаге мы получили 6×2=12, то суммируем 1+2).

Вычисляем остаток от деления полученной суммы на 10. Результат и будет контрольной цифрой. При проверке сумма должна делиться на 10 без остатка.

В качестве примера проверим правильность числа 49927398716:

Удваиваем каждую вторую цифру: (1×2) = 2, (8×2) = 16, (3×2) = 6, (2×2) = 4, (9×2) = 18.

Суммируем все цифры (цифры в скобках — произведения, полученные на предыдущем шаге): 6 + (2) + 7 + (1+6) + 9 + (6) + 7 + (4) + 9 + (1+8) + 4 = 70.

Остаток от деления 70 на 10 равен нулю — число введено верно.

#### Реализация на языке Python

Ниже представлен небольшой скрипт с подробными комментариями.

<details>
<summary>Реализация алгоритма Луна на языке Python</summary>

```
def Luhn(card):
    # Здесь хранится контрольная сумма
    checksum = 0
    # Номер карты переводится из строки в массив. Это нужно для дальнейшей итерации по каждой цифре
    cardnumbers = list(map(int, card))
    # Итерации по каждой цифре
    for count, num in enumerate(cardnumbers):
        # Т.к. счёт идёт с нуля, то если index чётный, значит число стоит на нечётной позиции
        if count % 2 == 0:
            buffer = num * 2
            # Если удвоенное число больше 9, то из него вычитается 9 и прибавляется к контрольной сумме
            if buffer > 9:
                buffer -= 9
            # Если нет, то сразу прибавляется к контрольной сумме
            checksum += buffer
        # Если число стоит на чётной позиции, то оно прибавляется к контрольной сумме
        else:
            checksum += num
    # Если контрольная сумма делится без остатка на 10, то номер карты правильный

    if checksum % 10 == 0:
      print(f'Номер карты {card} - корректный!')
    else:
      print(f'Номер карты {card} - некорректный!')


card = input('Введите номер карты! Номер должен состоять из 16 цифр!')

# Проверка на валидность введённого числа
if len(card) == 16 and card.isdigit() == True:
  Luhn(card)
else:
  print('Номер введён некорректно!')
```
</details>


<details>
<summary>Реализация алгоритма Луна на языке Ruby</summary>

```
def luhn(card)
  # Здесь хранится контрольная сумма
  checksum = 0
  # Номер карты переводится из строки в массив. Это нужно для дальнейшей итерации по каждой цифре
  card_numbers = card.chars.map(&:to_i)
  # Итерации по каждой цифре
  card_numbers.each_with_index do |num, count|
    # Т.к. счёт идёт с нуля, то если index чётный, значит число стоит на нечётной позиции
    if count % 2 == 0
      buffer = num * 2
      # Если удвоенное число больше 9, то из него вычитается 9 и прибавляется к контрольной сумме
      if buffer > 9
        buffer -= 9
      end
      # Если нет, то сразу прибавляется к контрольной сумме
      checksum += buffer
    # Если число стоит на чётной позиции, то оно прибавляется к контрольной сумме
    else
      checksum += num
    end
  end
  # Если контрольная сумма делится без остатка на 10, то номер карты правильный
  if checksum % 10 == 0
    puts "Номер карты #{card} - корректный!"
  else
    puts "Номер карты #{card} - некорректный!"
  end
end

puts 'Введите номер карты! Номер должен состоять из 16 цифр!'
card = gets.chomp

# Проверка на валидность введённого числа
if card.length == 16 && card.match?(/^\d+$/)
  luhn(card)
else
  puts 'Номер введён некорректно!'
end
```
</details>


