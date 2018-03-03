# from random import randrange
from collections import Counter
import numpy as np


def bot2():
     bot_sign2=randrange(3);
     if bot_sign2==0:
        print('Jojo выбрали КАМЕНЬ')
      
     elif bot_sign2==1:
        print('Jojo выбрали НОЖНИЦЫ')
           
     elif bot_sign2==2:
        print('Jojo выбрали БУМАГА')
          
     return bot_sign2;



def getCounterPik(sign):
    if sign==0 :
            counterPik = 2;
            print('\nБот выбрал БУМАГА')

    elif sign==1:
            counterPik =0;
            print('\nБот выбрал КАМЕНЬ')

    elif sign==2:
            counterPik =1;
            print('\nБот выбрал НОЖНИЦЫ')
    return counterPik;



def getContrContrPik(sign):
    if sign==0 :
            counterPik = 1;
            print('\nБот3 выбрал Ножницы')

    elif sign==1:
            counterPik =2;
            print('\nБот3 выбрал Бумага')

    elif sign==2:
            counterPik =0;
            print('\nБот3 выбрал Камень')
    return counterPik;


loose=0;
draws=0;

def bot3(prev_human_sign=[], prev_round_result=[], round_number=0):
    bot_sign=0;
   
    if round_number  < 0:
        bot_sign=randrange(3);
        print('\nБот3 рандом:{0}'.format(bot_sign));
    else:
        c=np.bincount(prev_human_sign)
        player_sign=np.arange(len(c))[c==c.max()].min();
        
        if prev_round_result[round_number]==1:
            bot_sign=getCounterPik(prev_human_sign[round_number]);
            draws = 0;
        elif prev_round_result[round_number]==2:
            bot_sign =getCounterPik(prev_human_sign[round_number]);
            draws += 1;
            if draws > 3 :
                bot_sign=getContrContrPik(prev_human_sign[round_number]);
                bot_sign=getContrContrPik(bot_sign);
        elif prev_round_result[round_number]==0:
                draws = 0;     
                      #bot_sign=prev_human_sign[round_number]
                bot_sign=getCounterPik(player_sign);
    return bot_sign;


def bot(prev_human_sign=[], prev_round_result=[], round_number=0):
    bot_sign=0;
   
    if round_number  < 0:
        bot_sign=randrange(3);
        print('\nБот рандом:{0}'.format(bot_sign));
    else:
        c=np.bincount(prev_human_sign)
        player_sign=np.arange(len(c))[c==c.max()].min();
        
        if prev_round_result[round_number]==2 or prev_round_result[round_number]==1:
            bot_sign=getCounterPik(prev_human_sign[round_number]);
            print("Контр-пик ничьи или поражения Бота");
        elif prev_round_result[round_number]==0:
                      print("Контр-пик победы Бота");
                      #bot_sign=prev_human_sign[round_number]
                      bot_sign=getCounterPik(player_sign);
    return bot_sign;

#prev_human_sign - жест, который человек показал на предыдущем раунде (раунд номер k-1), 
#prev_round_result – результат предыдущего (k-1-ого) раунда относительно человека, 
#round_number – номер текущего (k-ого) раунда,  
#bot_sign – выбранный ботом жест в текущем (k-ом) раунде.

def player():
    
    i=0;
    while i<=10:
        player_sign = int(input("Введите № жеста: "))
        if player_sign==0:
            print('\nВы выбрали КАМЕНЬ')
            break;
        elif player_sign==1:
            print('\nВы выбрали НОЖНИЦЫ')
            break;
        elif player_sign==2:
            print('\nВы выбрали БУМАГА')
            break;
        else:
            ++i;
            print('Вы ввели не правильное значение')
   
    return player_sign;



def menu():
    return('''Игра-камень,ножницы,бумага
################################
#       Коды для жестов:       # 
#        0 — камень            # 
#        1 — ножницы           # 
#        2 — бумага            # 
################################
    ''')
def game():

    historyPlayer=[];
    historyResultGame=[];
    


    print(menu());


    numberGame=0;
    countWin=0;
    countLoose=0;
    countDraw=0;


    while numberGame<10:
        print('№ раунда: {0} '.format(numberGame))
        resultPlayer=player()
        #resultPlayer=bot2()
        
        resultBot=bot(historyPlayer, historyResultGame, numberGame-1)
        
       
        historyPlayer.append(resultPlayer);
       
        if resultBot == resultPlayer: 
            print('НИЧЬЯ')
            historyResultGame.append(2)
            countDraw+=1;


        elif resultPlayer==1 and resultBot==2 or resultPlayer==0 and resultBot==1 or resultPlayer==2 and resultBot==0:
            print('Игрок победил')
            historyResultGame.append(1);
            
            countWin+=1;

        else: 
            historyResultGame.append(0)
            print('БОТ победил!!')
            countLoose+=1;

        numberGame+=1;
   
    print('Победы- {0}\n  Поражений - {1}\n Ничьи- {2}'.format(countWin, countLoose, countDraw));
   


if __name__ == "__main__":
    game();
