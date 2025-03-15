# aca_final_analitica-de-datos
Presentación final Aca analítica de datos
Código modelo en python para su ejecución en las librearias (pandas, numpy, matplotlib, seaborn, sklearn)


import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_absolute_error, mean_squared_error

# Cargar los datos
df = pd.read_csv("gym_data.csv")

# Exploración inicial
def explorar_datos(df):
    print("Primeras filas del dataset:")
    print(df.head())
    print("\nResumen estadístico:")
    print(df.describe())
    print("\nValores nulos:")
    print(df.isnull().sum())

def visualizar_datos(df):
    plt.figure(figsize=(10, 5))
    sns.boxplot(x='Hora', y='Cantidad', data=df, palette="coolwarm")
    plt.title("Distribución de personas en el gimnasio por hora")
    plt.xticks(rotation=45)
    plt.show()

    plt.figure(figsize=(10, 5))
    sns.boxplot(x='Día', y='Cantidad', data=df, palette="viridis")
    plt.title("Distribución de personas en el gimnasio por día de la semana")
    plt.xticks(rotation=45)
    plt.show()

# Preprocesamiento de datos
df = pd.get_dummies(df, columns=["Día", "Hora", "Mes"], drop_first=True)

# División de datos en entrenamiento y prueba
X = df.drop(columns=["Cantidad"])
y = df["Cantidad"]
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Entrenamiento del modelo
modelo = LinearRegression()
modelo.fit(X_train, y_train)

# Predicción
y_pred = modelo.predict(X_test)

# Evaluación del modelo
mae = mean_absolute_error(y_test, y_pred)
mse = mean_squared_error(y_test, y_pred)
rmse = np.sqrt(mse)

print(f"MAE: {mae:.2f}")
print(f"RMSE: {rmse:.2f}")

if __name__ == "__main__":
    explorar_datos(df)
    visualizar_datos(df)
