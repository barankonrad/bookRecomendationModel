### README.md

# System Rekomendacji Książek

## Przegląd

Ten projekt implementuje system rekomendacji książek za pomocą algorytmu K-Nearest Neighbors (KNN). System jest zaprojektowany do rekomendowania książek użytkownikom na podstawie ich historii ocen i wzorców ocen podobnych użytkowników. Zestawy danych użyte w tym projekcie pochodzą z [Book Recommendation Dataset na Kaggle](https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset/data).

## Zestawy danych

Projekt wykorzystuje trzy główne zestawy danych:
1. **Books Dataset (`df_books`)**:
    - **Rozmiar**: 271,360 wierszy, 8 kolumn
    - **Kolumny**:
        - `ISBN`: Unikalny identyfikator książki
        - `Book-Title`: Tytuł książki
        - `Book-Author`: Autor książki
        - `Year-Of-Publication`: Rok publikacji
        - `Publisher`: Wydawca książki
        - `Image-URL-S`: URL do małego obrazka okładki książki
        - `Image-URL-M`: URL do średniego obrazka okładki książki
        - `Image-URL-L`: URL do dużego obrazka okładki książki

2. **Users Dataset (`df_users`)**:
    - **Rozmiar**: 278,858 wierszy, 3 kolumny
    - **Kolumny**:
        - `User-ID`: Unikalny identyfikator użytkownika
        - `Location`: Lokalizacja użytkownika
        - `Age`: Wiek użytkownika

3. **Ratings Dataset (`df_ratings`)**:
    - **Rozmiar**: 1,149,780 wierszy, 3 kolumny
    - **Kolumny**:
        - `User-ID`: Unikalny identyfikator użytkownika
        - `ISBN`: Unikalny identyfikator książki
        - `Book-Rating`: Ocena przyznana przez użytkownika książce

## Przetwarzanie danych

### Czyszczenie i przygotowanie danych
1. **Usuwanie duplikatów**:
   Usunięcie duplikatów tytułów książek, aby zapewnić unikalność każdej książki.
2. **Liczba recenzji**:
   Obliczenie liczby recenzji dla każdej książki i użytkownika oraz odfiltrowanie tych, które mają mniej niż minimalna wymagana liczba recenzji.
3. **Ważona średnia ocena**:
   Obliczenie ważonej średniej oceny dla każdej książki na podstawie ocen użytkowników.
4. **Filtrowanie**:
   Filtrowanie danych ocen, aby uwzględnić tylko użytkowników i książki, które spełniają minimalne progi recenzji.

### Tworzenie macierzy rzadkiej
Używam rzadkiej macierzy ocen przy użyciu funkcji `coo_matrix` z `scipy.sparse`. Ta macierz reprezentuje oceny użytkowników dla książek, gdzie wiersze reprezentują użytkowników, a kolumny książki.

## Trenowanie modelu

Model K-Nearest Neighbors jest trenowany przy użyciu metryki **cosine similarity**. Dane treningowe są podzielone na zestawy treningowe i testowe, a model jest trenowany i estymowany na zestawie treningowym.

### Strojenie hiperparametrów
Różne kombinacje liczby sąsiadów i metryk odległości są testowane, aby znaleźć model o najlepszej wydajności na podstawie **RMSE**.

## Predykcja i rekomendacje

### Predykcja ocen
Funkcja jest zaimplementowana do przewidywania ocen dla danego użytkownika i książki poprzez znalezienie najbliższych sąsiadów i obliczenie ważonej sumy ich ocen.

### Rekomendacje
Funkcja generuje rekomendacje książek dla użytkownika poprzez identyfikację książek wysoko ocenianych przez podobnych użytkowników, ale jeszcze nie ocenianych przez użytkownika docelowego. Rekomendacje są rankowane na podstawie obliczonego wyniku.

## Wymagania

- pandas
- numpy
- scipy
- scikit-learn
- IPython

Aby zainstalować wymagane biblioteki, uruchom:
```bash
pip install -r requirements.txt
```

## Uruchamianie projektu

1. **Sklonuj repozytorium**:
   ```bash
   git clone https://github.com/barankonrad/bookRecomendationModel.git
   ```

2. **Upewnij się, że dane są dostępne**:
   Upewnij się, że zestawy danych (`books.csv`, `users.csv`, `ratings.csv`) są dostępne w katalogu `data`.

3. **Zainstaluj zależności**:
   Zainstaluj wymagane biblioteki wymienione w `requirements.txt`:
   ```bash
   pip install -r requirements.txt
   ```

4. **Uruchom projekt**:
   Otwórz notebook Jupyter lub uruchom skrypt Python, aby zobaczyć rekomendacje w akcji.
   ```bash
   jupyter notebook book_recommendation_system.ipynb
   ```

5. **Uzyskaj rekomendacje**:
   Użyj funkcji `get_recommends`, aby uzyskać rekomendacje książek dla użytkownika:
   ```python
   user_id = 254
   recommended_books = get_recommends(user_id)
   print(recommended_books)
   ```

6. **Uzyskaj oceny użytkownika**:
   Użyj funkcji `get_user_ratings`, aby uzyskać listę książek ocenionych przez użytkownika:
   ```python
   user_rated_books = get_user_ratings(user_id)
   print(user_rated_books)
   ```