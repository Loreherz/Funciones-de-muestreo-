# Funciones-de-muestreo-
import pandas as pd
import numpy as np
import random
import io

from google.colab import files
uploaded = files.upload()

econdata = pd.read_csv(io.BytesIO(uploaded["econdata (2).csv"]))
econdata.head()

Muestreo aleatorio simple 
aleat_8=econdata.sample(n=8)
aleat_8

prop_25 = econdata.sample(frac=.25)
prop_25.head()

Muestreo sistemático

def systematic_sampling(econdata,step):
    indexes = np.arange(0,len(econdata),step =step)
    systematic_sample =econdata.iloc[indexes]
    return systematic_sample

systematic_sample = systematic_sampling(econdata,3)
systematic_sample

Muestreo estratificado

econdata['estratificado'] = econdata ['delegacion'] + "," + econdata ['tipo']
(econdata['estratificado'].value_counts()/len(econdata)).sort_values(ascending=False)

def data_estratificada(econdata,nombres_columnas_estrat,valores_estrat,prop_estrat,random_state=None):
    df_estrat = pd.DataFrame(columns = econdata.columns)

    pos = -1
    for i in range(len(valores_estrat)):
      pos +=1
      if pos == len(valores_estrat) -1 :
        ratio_len = len(econdata)-len(df_estrat)
      else :
           ratio_len = int(len(econdata) * prop_estrat[i])

      df_filtrado = econdata[econdata[nombres_columnas_estrat]==valores_estrat[i]]
      df_temp = df_filtrado.sample(replace=True, n=ratio_len, random_state=random_state)

      df_estrat = pd.concat([df_estrat,df_temp])
    return df_estrat

valores_estrat = ['Cuautémoc,Hotel', 'Cuautémoc,Museo','Venustiano Carranza,Hotel', 'Cuauhtémoc,Mercado', 'Venustiano Carranza,Mercado']
prop_estrat = [0.5,0.2,0.1,0.1,0.1]
df_estrat = data_estratificada(econdata, 'estratificado',valores_estrat, prop_estrat,random_state=42)
df_estrat


    






