import streamlit as st
import time
import random

# Configuração inicial
def initialize_game(grid_size=20):
    snake = [[grid_size // 2, grid_size // 2]]
    direction = "RIGHT"
    fruit = [random.randint(0, grid_size - 1), random.randint(0, grid_size - 1)]
    score = 0
    return snake, direction, fruit, score

# Desenhar o jogo
def draw_game(snake, fruit, grid_size):
    st.write(f"Pontuação: {len(snake) - 1}")
    for row in range(grid_size):
        line = ""
        for col in range(grid_size):
            if [row, col] in snake:
                line += "🟩"  # Corpo da cobra
            elif [row, col] == fruit:
                line += "🍎"  # Fruta
            else:
                line += "⬜"  # Espaço vazio
        st.text(line)

# Atualizar a posição da cobra
def move_snake(snake, direction, fruit):
    head = snake[0].copy()
    if direction == "UP":
        head[0] -= 1
    elif direction == "DOWN":
        head[0] += 1
    elif direction == "LEFT":
        head[1] -= 1
    elif direction == "RIGHT":
        head[1] += 1
    snake.insert(0, head)
    if head == fruit:
        fruit = [random.randint(0, grid_size - 1), random.randint(0, grid_size - 1)]
    else:
        snake.pop()
    return snake, fruit

# Verificar colisões
def check_collision(snake, grid_size):
    head = snake[0]
    # Bateu na parede
    if head[0] < 0 or head[0] >= grid_size or head[1] < 0 or head[1] >= grid_size:
        return True
    # Bateu no próprio corpo
    if head in snake[1:]:
        return True
    return False

# Código principal
st.title("Jogo da Cobrinha")
st.sidebar.title("Configurações")
grid_size = st.sidebar.slider("Tamanho do grid", 10, 30, 20)
speed = st.sidebar.slider("Velocidade (segundos por movimento)", 0.1, 1.0, 0.3)

# Inicialização
if "snake" not in st.session_state:
    snake, direction, fruit, score = initialize_game(grid_size)
    st.session_state.snake = snake
    st.session_state.direction = direction
    st.session_state.fruit = fruit
    st.session_state.score = score

# Capturar teclas
key = st.sidebar.radio("Controles", ["UP", "DOWN", "LEFT", "RIGHT"])
st.session_state.direction = key

# Loop do jogo
while True:
    st.session_state.snake, st.session_state.fruit = move_snake(
        st.session_state.snake, st.session_state.direction, st.session_state.fruit
    )
    if check_collision(st.session_state.snake, grid_size):
        st.write("💀 Você perdeu!")
        st.write(f"Pontuação final: {len(st.session_state.snake) - 1}")
        break
    draw_game(st.session_state.snake, st.session_state.fruit, grid_size)
    time.sleep(speed)
    st.experimental_rerun()
