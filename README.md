# Establecemos valores iniciales para la ventana del juego
WIDTH = 800
HEIGHT = 600
window = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Dodge Path")

# Establecemos valores iniciales para el personaje
player_width = 50
player_height = 50
player_x = WIDTH / 2 - player_width / 2
player_y = HEIGHT - player_height - 50
player_speed = 10

# Creamos una lista de obstáculos
obstacles = []

# Creamos una variable para llevar la puntuación del jugador
score = 0

# Creamos variables para los colores
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)

# Creamos la clase para los obstáculos
class Obstacle:
    def __init__(self):
        self.width = random.randint(50, 100)
        self.height = 50
        self.x = random.randint(0, WIDTH - self.width)
        self.y = 0 - self.height
        self.speed = random.randint(5, 10)

    def move(self):
        self.y += self.speed

    def draw(self):
        pygame.draw.rect(window, RED, (self.x, self.y, self.width, self.height))

# Creamos la función para generar obstáculos
def generate_obstacle():
    obstacle = Obstacle()
    obstacles.append(obstacle)

# Creamos la función para detectar colisiones
def detect_collision():
    global score
    for obstacle in obstacles:
        if (obstacle.y + obstacle.height) > player_y:
            if (obstacle.x < (player_x + player_width)) and ((obstacle.x + obstacle.width) > player_x):
                return True
            else:
                score += 10
    return False

# Creamos el loop principal del juego
running = True
clock = pygame.time.Clock()
while running:

    # Manejamos eventos del teclado y de salida
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT and player_x > 0:
                player_x -= player_speed
            if event.key == pygame.K_RIGHT and player_x < (WIDTH - player_width):
                player_x += player_speed

    # Generamos un nuevo obstáculo aleatorio cada cierto tiempo
    if len(obstacles) < 5:
        generate_obstacle()

    # Movemos y dibujamos los obstáculos
    for obstacle in obstacles:
        obstacle.move()
        obstacle.draw()

    # Dibujamos al personaje
    pygame.draw.rect(window, WHITE, (player_x, player_y, player_width, player_height))

    # Detectamos colisiones
    if detect_collision():
        running = False

    # Dibujamos la puntuación
    font = pygame.font.SysFont(None, 32)
    text = font.render("Score: " + str(score), True, WHITE)
    window.blit(text, (10, 10))

    # Actualizamos la ventana y establecemos el framerate del juego
    pygame.display.update()
    clock.tick(60)

# Cerramos Pygame y salimos del programa
pygame.quit()
quit()
