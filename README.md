# game-1
import pgzrun
from random import randint
WIDTH=1000
HEIGHT=700
flying=Actor("hero")
flying.y=HEIGHT/2
flying.x=WIDTH/2
standing=Actor("standing")
standing.y=HEIGHT/2
standing.x=WIDTH/2
earth=Actor("earth")
earth.y=HEIGHT/2
end=Actor("game over")
end.x=WIDTH/2
end.y=HEIGHT/2
ufos2=[]
lasers=[]
ufos=[]
game_over=False
def draw():
    screen.blit("space",(0,0))
    if not game_over:
        if keyboard.D:
            flying.draw()
        else:
            standing.draw()
        earth.draw()
        for laser in lasers:
            laser.draw()
        for ufo1 in ufos: 
            ufo1.draw()
        for ufo2 in ufos2:
            ufo2.draw()
    elif game_over:
        end.draw()

def update():
    if not game_over:
        move_hero()
        move_laser()
        move_ufo1()
        move_ufo2()
        laser_collision()
        death()
def move_ufo1():
    global earth
    global ufo1
    for ufo1 in ufos:
        if earth.right<=0:
            ufo1.x-=0.3
            if keyboard.D:
                ufo1.x-=1.6
def move_ufo2():
    global earth
    global ufo2
    for ufo2 in ufos2:
        if earth.right<=0:
            ufo2.x-=0.4
            if keyboard.D:
                ufo2.x-=2

def on_mouse_down(button):
    if button==button.LEFT:
        create_laser()
def move_laser():
    global laser
    for laser in lasers:
        laser.x+=50
        if laser.left>WIDTH:
            lasers.remove(laser)

def move_hero():
    global flying
    global earth
    if flying.top<0:
        if keyboard.W:
            flying.y-=0.001
        elif keyboard.S:
            flying.y+=1
        standing.y=flying.y
    elif flying.bottom>HEIGHT:
        if keyboard.W:
            flying.y-=1
        elif keyboard.S:
            flying.y+=.001
            standing.y=flying.y
    elif flying.top>=0 or flying.bottom<=HEIGHT:
        if keyboard.W:
            flying.y-=10
        elif keyboard.S:
            flying.y+=10
        standing.y=flying.y
    if keyboard.D:
        earth.x-=5

def on_key_down(key):
    if key==keys.KP0:
        create_laser()
def create_ufo():
    ufo1=Actor("ufo")
    ufo1.y=randint(0,HEIGHT)
    ufo1.left=WIDTH
    ufos.append(ufo1)
clock.schedule_interval(create_ufo,1.0)
def create_ufo2():
    ufo2=Actor("ufo1")
    ufo2.y=randint(0,HEIGHT)
    ufo2.left=WIDTH
    ufos2.append(ufo2)
clock.schedule_interval(create_ufo2,4.0)

def create_laser():
    laser=Actor("laser")
    laser.y=flying.y
    laser.left=flying.left
    lasers.append(laser)

def laser_collision():
    for laser in lasers:
        for ufo1 in ufos:
            if laser.colliderect(ufo1):
                if ufo1 in ufos:
                    ufos.remove(ufo1)
                if laser in lasers:
                    lasers.remove(laser)
        for ufo2 in ufos2:
            if laser.colliderect(ufo2):
                if ufo2 in ufos2:
                    ufos2.remove(ufo2)
                if laser in lasers:
                    lasers.remove(laser)
                        
def death():
    global game_over
    for ufo1 in ufos:
        for ufo2 in ufos2:
            if flying.colliderect(ufo1) or ufo1.right<HEIGHT/2:
                game_over=True
            if flying.colliderect(ufo2) or ufo2.right<HEIGHT/2:
                game_over=True


pgzrun.go()
