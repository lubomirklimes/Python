# Typy neuronových sítí (MLP, CNN, RNN, Transformer)

Tato kapitola vysvětluje základní stavební prvky neuronových sítí a kdy deep learning dává smysl.

## 1. Concept

Neuronová síť je model složený z vrstev neuronů. Každý neuron počítá vážený součet vstupů, aplikuje aktivační funkci a posílá výstup dál. V praxi je důležité zvolit správný typ architektury pro daný problém.

Klíčové pojmy:

- vrstvy: vstupní, skryté, výstupní
- activation function: nelinearita (např. ReLU, sigmoid)
- loss function: jak moc je predikce špatně
- optimizer: jak upravit váhy (např. SGD, Adam)
- backpropagation: výpočet gradientů pro update vah
- epoch: jeden průchod celým tréninkovým datasetem
- batch: menší část dat pro jeden update
- learning rate: krok změny vah

Typické rodiny sítí:

- MLP: základní plně propojené sítě, vhodné hlavně pro tabulková data
- CNN: konvoluční sítě, standard pro obrazová data
- RNN/LSTM/GRU: sekvenční modely pro časové řady a text
- Transformer: moderní architektura pro jazyk i obraz (vision transformer)

Deep learning má smysl u složitých dat (obraz, text, audio) a větších datasetů. Je zbytečný, když máte malá tabulková data, kde jednodušší model funguje stejně dobře.

## 2. Example

```python
import tensorflow as tf

# MLP pro tabulková data
mlp = tf.keras.Sequential(
    [
        tf.keras.layers.Input(shape=(16,)),
        tf.keras.layers.Dense(64, activation="relu"),
        tf.keras.layers.Dense(32, activation="relu"),
        tf.keras.layers.Dense(1, activation="sigmoid"),
    ]
)

# CNN pro obrazová data
cnn = tf.keras.Sequential(
    [
        tf.keras.layers.Input(shape=(128, 128, 3)),
        tf.keras.layers.Conv2D(32, 3, activation="relu"),
        tf.keras.layers.MaxPooling2D(),
        tf.keras.layers.Conv2D(64, 3, activation="relu"),
        tf.keras.layers.MaxPooling2D(),
        tf.keras.layers.Flatten(),
        tf.keras.layers.Dense(128, activation="relu"),
        tf.keras.layers.Dense(5, activation="softmax"),
    ]
)

print("MLP architektura")
mlp.summary()
print("CNN architektura")
cnn.summary()
```

## 3. When to use

* Když řešíte nestrukturovaná data a klasické modely nestačí.
* Když máte dost dat i výpočetních zdrojů pro stabilní trénink.
* Když lze obchodně obhájit vyšší složitost a provozní náklady.

## 4. Common mistakes

* ❌ Nasazení deep learningu bez baseline porovnání s jednoduššími modely.
* ❌ Ignorování overfittingu a absence validace.
* ❌ Špatně nastavený learning rate, který brání konvergenci.

---

<details>
<summary>Optional: Deep dive</summary>

Backpropagation není "magie", ale aplikace řetězového pravidla. V praxi je největší riziko spíš kvalita dat, bias a špatná evaluace než samotná implementace tréninkového algoritmu.

</details>

---

**Další krok:** [TensorFlow / Keras workflow](TensorFlow.md)
