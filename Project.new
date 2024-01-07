import pygame
import random

pygame.init()

FPS = 60 # 
width_height = (1000, 500) # tuple ширини та висоти ігрового вікна
button_w_h = (100, 200) # tuple ширини та висоти кнопки
step = 10 # відстань між кнопками 
font_size = 20 # розмір шрифта "Натисни"
font_size_lable = 40 # розмір шрифта написів та цифр

list_color = [(149,196,194),(255,0,0), (255,255,0), (0,0,0), (0, 255, 0)] # список всіх кольорів 
list_button = list() # список всіх кнопок

clock = pygame.time.Clock()
# задаємо вікно з параметрами ширини та висоти
win = pygame.display.set_mode(width_height)
# задаємо назву вікну
pygame.display.set_caption("Fast Clicker!")
# Клас що відповідає за створення всіх кнопок в грі
class Area:
    def __init__(self, x = None, y = None, width = None, height = None, color = None):
        self.X = x
        self.Y = y
        self.WIDTH = width
        self.HEIGHT = height
        self.COLOR = color
        self.rectangle = pygame.Rect(self.X, self.Y, self.WIDTH, self.HEIGHT)
        self.SELECT = False 
        self.FONT = pygame.font.Font(None, font_size)
        self.TEXT = self.FONT.render("НАТИСНИ", True, list_color[3]) 
    # метод, що промальовує rect об'єкт та напис, на поверхні картки
    def blit_button(self,win):
        pygame.draw.rect(win, self.COLOR, self.rectangle)
        pygame.draw.rect(win, list_color[1], self.rectangle, width= 4)
        # Якщо у кнопки значення SELECT = True, то на цій кнопці з'являється напис "НАТИСНИ"
        if self.SELECT:
            win.blit(self.TEXT, (self.X + self.WIDTH//2 - font_size * 1.5, self.Y + self.HEIGHT// 2 - font_size/2))
# Клас, що відповідає за стоврення написів рахунку гри
class Lable:
    def __init__(self, x = width_height[0] - font_size_lable*10 , y = font_size_lable):
        self.X = x
        self.Y = y
        self.FONT = pygame.font.Font(None, font_size_lable)
        self.SCORE = 10
        self.TEXT = self.FONT.render("Рахунок: " + str(self.SCORE), True, list_color[3])
    # метод, що промальовує текст рахунку гри 
    def blit_lable(self, win):
        self.TEXT = self.FONT.render("Рахунок: " + str(self.SCORE), True, list_color[3])
        win.blit(self.TEXT, (self.X, self.Y))
# функція, що створює об'єкти кнопок та записує до списку list_button
def create_rect(count):
    x = width_height[0] // 2 - ((button_w_h[0] + step) * count/2 - step//2)
    y = width_height[1] // 2 - button_w_h[1] // 2
    for el in range(count):
        button = Area(
            x = x,
            y = y,
            width = button_w_h[0],
            height= button_w_h[1],
            color= list_color[2]
        )
        list_button.append(button)
        x += button_w_h[0] + step

create_rect(4)
# Основна функція гри
def run_game():

    count_button_select = 0 # змінна, що відповідає за тривалість відображення тексту на кнопці
    count_color = 0 # змінна, що відповідає за тривалість зміни кольору кнопки
    score = Lable() # об'єкт рахунку

    game = True
    
    while game:

        win.fill(list_color[0])
        # умова, що задає жовтий кольор, якщо кнопка була не натиснута______________
        if count_color == 0:
            for button in list_button:
                button.COLOR = list_color[2]
            count_color = -1
        else:
            count_color -= 1
        # ___________________________________________________________________________
        # Умова, що відповідає за час відображення напису на рандомній кнопці
        if count_button_select == 0:
            for button in list_button:
                # вкладена умова, що прибирає можливу помилку, коли напис може з'явитися на двох або більше кнопках одночасно 
                if button.SELECT:
                    button.SELECT = False
                    score.SCORE -= 1
            random_index = random.randint(0,len(list_button)-1)
            list_button[random_index].SELECT = True
            count_button_select = 50
        else:
            count_button_select -= 1 
        # Промальовуємо всі кнопки на екрані
        for button in list_button:
            button.blit_button(win)
        # промальовуємо напис рахунку
        score.blit_lable(win)
        #   
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game = False
            if event.type == pygame.MOUSEBUTTONDOWN and event.button == 1:
                x, y = event.pos
                for button in list_button:
                    if button.rectangle.collidepoint(x, y):
                        # перевіряємо, якщо кнопка натиснута і є напис, то додаємо до рахунку одиницю
                        if button.SELECT:
                            button.COLOR = list_color[4]
                            score.SCORE += 1
                        # якщо кнопка натиснута і напису немає, то віднімаємо від рахунку одиницю
                        else:
                            button.COLOR = list_color[1]
                            score.SCORE -= 1
                        count_color = 20
                        button.SELECT = False
        # якщо рахунок = 0, гру завершено
        if score.SCORE == 0:
            game = False

        clock.tick(FPS)        
        pygame.display.flip()
run_game()
