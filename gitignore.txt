import pygame
import sys

# Inicializar Pygame
pygame.init()

# Configurar la pantalla
pantalla = pygame.display.set_mode((600, 400))
pygame.display.set_caption('Ping Pong')

# Crear las raquetas
raqueta_izquierda = pygame.Rect(20, 150, 10, 100)
raqueta_derecha = pygame.Rect(570, 150, 10, 100)

# Crear la bola
bola = pygame.Rect(300, 200, 10, 10)

# Establecer la velocidad de la bola
velocidad_x = 5
velocidad_y = 5

# Establecer la puntuación
puntuacion_izquierda = 0
puntuacion_derecha = 0

# Bucle principal del juego
while True:

    # Manejar eventos
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            sys.exit()

    # Mover la raqueta izquierda
    teclas_pulsadas = pygame.key.get_pressed()
    if teclas_pulsadas[pygame.K_UP]:
        raqueta_izquierda.y -= 5
    if teclas_pulsadas[pygame.K_DOWN]:
        raqueta_izquierda.y += 5

    # Mover la raqueta derecha
    if teclas_pulsadas[pygame.K_w]:
        raqueta_derecha.y -= 5
    if teclas_pulsadas[pygame.K_s]:
        raqueta_derecha.y += 5

    # Mover la bola
    bola.x += velocidad_x
    bola.y += velocidad_y

    # Rebotar la bola en las paredes
    if bola.x < 0 or bola.x > 600:
        velocidad_x *= -1

    if bola.y < 0 or bola.y > 400:
        velocidad_y *= -1

    # Rebotar la bola en las raquetas
    if raqueta_izquierda.colliderect(bola):
        velocidad_x *= -1
    if raqueta_derecha.colliderect(bola):
        velocidad_x *= -1

    # Comprobar si la bola ha salido de la pantalla
    if bola.x < 0:
        puntuacion_derecha += 1
    if bola.x > 600:
        puntuacion_izquierda += 1

    # Comprobar si alguien ha ganado
    if puntuacion_izquierda == 10:
        print('¡Ganó el jugador izquierdo!')
        sys.exit()
    if puntuacion_derecha == 10:
        print('¡Ganó el jugador derecho!')
        sys.exit()

    # Dibujar la pantalla
    pantalla.fill((0, 0, 0))
    pygame.draw.rect(pantalla, (255, 255, 255), raqueta_izquierda)
    pygame.draw.rect(pantalla, (255, 255, 255), raqueta_derecha)
    pygame.draw.rect(pantalla, (255, 255, 255), bola)

    # Mostrar la puntuación
    fuente = pygame.font.Font(None, 36)
    texto_puntuacion_izquierda = fuente.render(str(puntuacion_izquierda), True, (255, 255, 255))
    texto_puntuacion_derecha = fuente.render(str(puntuacion_derecha), True, (255, 255, 255))
    pantalla.blit(texto_puntuacion_izquierda, (10, 10))
    pantalla.blit(texto_puntuacion_derecha, (580, 10))

    # Actualizar la pantalla
    pygame.display.flip()

# Finalizar Pygame
pygame.quit()
