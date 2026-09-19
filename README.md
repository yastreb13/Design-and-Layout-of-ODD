# Оптимизация компоновки редуктора

Минимизация габаритной площади корпуса методом штрафных функций в PyTorch.
<img width="523" height="313" alt="image" src="https://github.com/user-attachments/assets/eb5a7ecb-aa8e-49b9-98c9-2fedbc002ffd" />
Можно поменять под ваш редуктор! Советую делать несколько запусков с рандомными весами, эпох до 1000. Упомяните меня в своем курсаче).
### 1. Кинематика осей

Параметры: theta = [alpha, beta, gamma]

* O1 = (0, 0)
* O2 = (x1 + 15 * cos(alpha), y1 + 15 * sin(alpha))
* O3 = (x2 + 20 * cos(beta), y2 + 20 * sin(beta))
* O4 = (x3 + 30 * cos(gamma), y3 + 30 * sin(gamma))
* Рейка: X от (x4 + 20) до (x4 + 28), Y от (y4 - H/2) до (y4 + H/2)

### 2. Целевая функция

* Width = max(X_all) - min(X_all)
* Height = max(Y_all) - min(Y_all)
* Area = Width * Height
X_all, Y_all учитывают двигатель (35x42), колеса (R2=10, R3=15, R4=25) и рейку.

### 3. Ограничения (штраф = ReLU(зазор)^2)

* d(O1, O3) >= 24
* d(O2, O4) >= 39
* d(O1, O4) >= 20 + hypot(17.5, 21) + 4
* Зазор двигатель-рейка: max(dx, dy) >= 4
* Loss = Area + 3000 * sum(штрафы)

### 4. Оптимизация

* PyTorch (autograd)
* Adam(lr=0.03), 10000 шагов
