# Game development

Tato kapitola představuje Pygame jako vstupní bod do vývoje her v Pythonu.

## 1. Concept

Pygame je knihovna pro 2D hry, zpracování vstupu, vykreslování a jednoduchý game loop. Je vhodná pro výuku, prototypy a menší hry. Python zde umožňuje rychle tvořit mechaniky a iterovat.

Pro velké komerční 3D projekty Python obvykle není hlavní volba enginu, ale stále je užitečný pro tooling, asset pipeline nebo skriptování.

## 2. Example

```python
import pygame

pygame.init()
screen = pygame.display.set_mode((640, 360))
clock = pygame.time.Clock()

x = 50
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    x = (x + 2) % 640
    screen.fill((30, 30, 30))
    pygame.draw.circle(screen, (80, 200, 120), (x, 180), 20)
    pygame.display.flip()
    clock.tick(60)

pygame.quit()
```

## 3. When to use

* Když se učíte principy herní smyčky, kolizí a vstupních událostí.
* Když tvoříte 2D prototyp nebo vzdělávací hru.

## 4. Common mistakes

* ❌ Chybějící limit FPS - hra pak běží odlišně na různém hardware.
* ❌ Míchání všech systémů do jednoho souboru bez architektury.

---

<details>
<summary>Optional: Deep dive</summary>

I u malých her pomůže oddělit update logiku, render a správu assetů. Pokud projekt roste, vyplatí se komponentový přístup a jednoduchý scene management.

</details>

---

**Další krok:** [CLI tools](CLI_tools.md)
