import numpy as np
import os
#--------------------------------------------------------
#----------------ВИСЕЛИЦА--------------------
#список состояний игры с многострочной графикой
#[''' ''', ''' ''', ]
#
#    +-----+
#    |     |
#          |
#          |
#          |
#          |
#  ==========
pics = [
  '''
  +-----+
  |     |
        |
        |
        |
        |
==========''', '''
  +-----+
  |     |
  O     |
        |
        |
        |
==========''', '''
  +-----+
  |     |
  O     |
  |     |
        |
        |
==========''', '''
  +-----+
  |     |
  O     |
 /|     |
        |
        |
==========''', '''
  +-----+
  |     |
  O     |
 /|\    |
        |
        |
==========''', '''
  +-----+
  |     |
  O     |
 /|\    |
 /      |
        |
==========''', '''
  +-----+
  |     |
  O     |
 /|\    |
 / \    |
        |
=========='''
]

#словарь - список из разных слов (split)
#['','','']
words = 'Указатель Радуга Мармелад Поиск Прятки Сторож Копейка Леопард Аттракцион Дрессировка Ошейник Карамель Водолаз Защита Батарея Решётка Квартира Дельфинарий Непогода Вход Полиция Перекрёсток Башня Стрелка Градусник Бутылка Щипцы Наволочка Павлин Карточка Записка Лестница Переулок Сенокос Рассол Закат Сигнализация Журнал Заставка Тиранозавр Микрофон Прохожий Квитанция Пауза Новости Скарабей Серебро Творог Осадок Песня Корзина Сдача Овчарка Хлопья Телескоп Микроб Угощение Экскаватор Письмо Пришелец Айсберг Пластик Доставка Полка Билет Вторник Льдина Интерес Троллейбус Футболист Лисёнок Пример Баклажан Лягушка Джокер Котлета Накидка Дикобраз Барбарис Работник Кристалл Доспехи Халва Велосипед Крючок Кочка Черепаха Петля Осень Яйцо'.lower(
).split()


#функция которая генерирует случайное слово (на вход словарь, на выход слово)
def word_creating(words):
  w1 = np.random.randint(1, len(words) - 1, dtype=int)
  return words[w1]


#функция для отображения текущего состояния игры и строки с разгадываемым словом (на вход: графика, неправильные буквы, правильные буквы, слово) 
#    +-----+
#    |     |
#          |
#          |
#          |
#          |
#  ==========
#  * * * * * * (*******)
#  missed_letters

#функция для проверки того, что ты ввел (на вход - предыдущие введенные буквы и правильные и неправильные)
#внутри вводишь новую букву (проверка на то, символ ли, была ли раньше, кирилица ли или нет?)


def check(guess, tried_letters):
  alphabet = 'абвгдеёжзийклмнопрстуфхцчшщъыьэюя'
  while True:
    if (len(guess) == 1) & (guess in alphabet) & (guess not in tried_letters):
      break
    else:
      print('Введите подходящую букву кириллицы')
      guess = input().lower()
#  good_guess = guess
  return guess


#функция которая спрашивает сыграть ли еще раз
def again():
  while True:
    try:
      answer = int(input("Начать заново? (1 - да, 0 - нет)"))
      return answer
    except:
      print('Вы ошиблись в вводе')


#--------основная программа--------
print('----В И С Е Л И Ц А----')
missed_letters = ''
guessed_letters = ''
endofgame = False
more_play = 0
flag2 = 0

while True:
  if (more_play == 0) & (flag2 != 0):
    break
  secret_word = word_creating(words)
  symbols = '*'*len(secret_word)
  more_play = 0
  flag2 = 0
  missed_letters = ''
  guessed_letters = ''
  print(pics[0])
  print(symbols)
  print('Неправильные буквы: ', missed_letters)

  
  while True:
    if (len(missed_letters) != 6) & (len(guessed_letters) != len(secret_word)):

      #debug
      #print('missed: ', missed_letters)
      #print('guessed: ', guessed_letters)
      #print('word: ', secret_word)
      
      print('Введите подходящую букву кириллицы')
      guess = input().lower()
      guess = check(guess, missed_letters + guessed_letters)


      if (guess in secret_word):
        guessed_letters = guessed_letters + guess
        print(pics[flag2])        
        replace_count = secret_word.count(guess)
        while replace_count > 0:
          tmp = list(symbols) #разбили строку на символьный массив
          tmp[secret_word.find(guess)] = guess #заменили нужный элемент на букву
          symbols = "".join(tmp) #собрали в строку обратно
          replace_count -= 1
        
        print(symbols)
        print('Неправильные буквы: ', missed_letters)
      else:
        missed_letters = missed_letters + guess
        flag2 = flag2 + 1
        print(pics[flag2])
        print(symbols)
        print('Неправильные буквы: ', missed_letters)

    else:
      if len(missed_letters) == 6:
        print("Вы проиграли")
        more_play = again()
        print(secret_word)
      else:
        print("Вы выиграли!")
        more_play = again()
      break
