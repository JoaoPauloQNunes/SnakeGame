# Bibliotecas
import pygame
import random
import time

# Organiza e gerencia as coordenadas X e Y
class Coordenada:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def atualizar(self, x, y):
        self.x = x
        self.y = y

    def obter(self):
        return (self.x, self.y)

# Organiza e gerencia a tela do jogo
class TelaJogo:
    def __init__(self, largura, altura):
        # Inicializa todos os módulos do Pygame e coloca o título do jogo
        pygame.init()
        pygame.display.set_caption("Jogo Snake Python OO")
        
        # Configuraçoes da tela e a taxa de atualização do jogo 
        self.largura = largura
        self.altura = altura
        self.tela = pygame.display.set_mode((self.largura, self.altura))
        self.relogio = pygame.time.Clock()

        # Cores no padrão RGB (Vermelho, Verde, Azul)
        self.preto = (0, 0, 0)
        self.branco = (255, 255, 255)
        self.vermelho = (255, 0, 0)
        self.verde = (0, 255, 0)
        self.azul = (0, 0, 225)

    def atualizar_tela(self):
        pygame.display.update()

    def cor_tela(self):
        self.tela.fill(self.preto)

    # Desenha os quadrados usados no resto do game 
    def desenhar_quadrado(self, cor, pos):
        pygame.draw.rect(self.tela, cor, pos)

    # Usado como configuração para textos 
    def textos(self, texto, pos):
        fonte = pygame.font.SysFont("Helvetica", 35)
        superficie_texto = fonte.render(texto, True, self.vermelho)
        self.tela.blit(superficie_texto, pos)
        
    # Taxa de atualização
    def tick(self, velocidade):
        self.relogio.tick(velocidade)

# Organiza e gerencia a cobrinha
class Cobra(Coordenada):
    def __init__(self, largura_tela, altura_tela, tam_quadrado):
        super().__init__(largura_tela / 2, altura_tela / 2)
        self.largura_tela = largura_tela
        self.altura_tela = altura_tela
        self.tam_quadrado = tam_quadrado
        self.velocidade_x = 0
        self.velocidade_y = 0
        self.tam_cobra = 1
        self.pixels = []

    def mover(self):
        self.x += self.velocidade_x
        self.y += self.velocidade_y
        self.pixels.append([self.x, self.y])
        if len(self.pixels) > self.tam_cobra:
            del self.pixels[0]

    # Muda a velocidade da cobrinha de acordo com a tecla usada
    def velocidade_movimento(self, tecla):
        if tecla == pygame.K_DOWN and self.velocidade_y == 0:
            self.velocidade_x = 0
            self.velocidade_y = self.tam_quadrado
        elif tecla == pygame.K_UP and self.velocidade_y == 0:
            self.velocidade_x = 0
            self.velocidade_y = -self.tam_quadrado
        elif tecla == pygame.K_RIGHT and self.velocidade_x == 0:
            self.velocidade_x = self.tam_quadrado
            self.velocidade_y = 0
        elif tecla == pygame.K_LEFT and self.velocidade_x == 0:
            self.velocidade_x = -self.tam_quadrado
            self.velocidade_y = 0

    # Desenha a cobrinha
    def desenhar(self, tela, cor):
        for pixel in self.pixels:
            tela.desenhar_quadrado(cor, [pixel[0], pixel[1], self.tam_quadrado, self.tam_quadrado])

    def colidir_borda(self):
        return self.x < 0 or self.x >= self.largura_tela or self.y < 0 or self.y >= self.altura_tela

    def colidir_corpo(self):
        for pixel in self.pixels[:-1]:
            if pixel == [self.x, self.y]:
                return True
        return False

# Organiza e gerencia a comida
class Comida(Coordenada):
    def __init__(self, largura_tela, altura_tela, tam_quadrado):
        super().__init__(0, 0)
        self.largura_tela = largura_tela
        self.altura_tela = altura_tela
        self.tam_quadrado = tam_quadrado
        self.comida_aleatoria()

    # Gera as comidas aleatoriamente 
    def comida_aleatoria(self):
        self.x = round(random.randrange(0, self.largura_tela - self.tam_quadrado) / float(self.tam_quadrado)) * float(self.tam_quadrado)
        self.y = round(random.randrange(0, self.altura_tela - self.tam_quadrado) / float(self.tam_quadrado)) * float(self.tam_quadrado)

    # Desenha as comidas 
    def desenhar(self, tela, cor):
        tela.desenhar_quadrado(cor, [self.x, self.y, self.tam_quadrado, self.tam_quadrado])

# Organiza e gerencia a a parte principal do jogo
class JogoSnake:
    def __init__(self):
        self.largura, self.altura = 800, 600  
        self.tam_quadrado = 20
        self.velocidade_jogo = 15
        self.tela = TelaJogo(self.largura, self.altura)
        self.cobra = Cobra(self.largura, self.altura, self.tam_quadrado)
        self.comida = Comida(self.largura, self.altura, self.tam_quadrado)

    def rodar_jogo(self):
        fim_jogo = False

        # Enquanto fim_jogo for falso roda o jogo
        while not fim_jogo:
            self.tela.cor_tela()

            for evento in pygame.event.get():
                if evento.type == pygame.QUIT:
                    fim_jogo = True
                elif evento.type == pygame.KEYDOWN:
                    self.cobra.velocidade_movimento(evento.key)
            
            # Se a cobrinha colidir na borda ou no corpo fim_jogo se torna verdade
            if self.cobra.colidir_borda() or self.cobra.colidir_corpo():
                fim_jogo = True

            self.cobra.mover()

            if self.cobra.x == self.comida.x and self.cobra.y == self.comida.y:
                self.cobra.tam_cobra += 1
                self.comida.comida_aleatoria()

            self.comida.desenhar(self.tela, self.tela.verde)
            self.cobra.desenhar(self.tela, self.tela.branco)

            # Desenha pontuação
            self.tela.textos(f"Pontos: {self.cobra.tam_cobra - 1}", [10, 10])

            self.tela.atualizar_tela()
            self.tela.tick(self.velocidade_jogo)
        
        self.fim_de_jogo()

    # Exibe a mensagem de fim de jogo e a pontuação final e fecha o jogo.
    def fim_de_jogo(self):
        self.tela.textos(f"Fim de jogo :( Pontuação final: {self.cobra.tam_cobra - 1}", [self.largura // 2 - 200, self.altura // 2 - 50])
        self.tela.atualizar_tela()
        time.sleep(5)

        pygame.quit()

def main():
    jogo = JogoSnake()
    jogo.rodar_jogo()

# inicia o loop principal - Chama a função main()
main()