```python
pip install Altair
```

    Collecting Altair
      Downloading altair-4.2.0-py3-none-any.whl (812 kB)
         -------------------------------------- 812.8/812.8 kB 2.3 MB/s eta 0:00:00
    Requirement already satisfied: pandas>=0.18 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from Altair) (1.4.4)
    Requirement already satisfied: jsonschema>=3.0 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from Altair) (4.16.0)
    Requirement already satisfied: jinja2 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from Altair) (2.11.3)
    Requirement already satisfied: toolz in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from Altair) (0.11.2)
    Requirement already satisfied: entrypoints in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from Altair) (0.4)
    Requirement already satisfied: numpy in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from Altair) (1.21.5)
    Requirement already satisfied: pyrsistent!=0.17.0,!=0.17.1,!=0.17.2,>=0.14.0 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from jsonschema>=3.0->Altair) (0.18.0)
    Requirement already satisfied: attrs>=17.4.0 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from jsonschema>=3.0->Altair) (21.4.0)
    Requirement already satisfied: pytz>=2020.1 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from pandas>=0.18->Altair) (2022.1)
    Requirement already satisfied: python-dateutil>=2.8.1 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from pandas>=0.18->Altair) (2.8.2)
    Requirement already satisfied: MarkupSafe>=0.23 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from jinja2->Altair) (2.0.1)
    Requirement already satisfied: six>=1.5 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from python-dateutil>=2.8.1->pandas>=0.18->Altair) (1.16.0)
    Installing collected packages: Altair
    Successfully installed Altair-4.2.0
    Note: you may need to restart the kernel to use updated packages.
    


```python
pip install snscrape
```

    Collecting snscrape
      Downloading snscrape-0.4.3.20220106-py3-none-any.whl (59 kB)
         ---------------------------------------- 59.1/59.1 kB 1.1 MB/s eta 0:00:00
    Requirement already satisfied: filelock in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from snscrape) (3.6.0)
    Requirement already satisfied: lxml in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from snscrape) (4.9.1)
    Requirement already satisfied: beautifulsoup4 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from snscrape) (4.11.1)
    Requirement already satisfied: requests[socks] in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from snscrape) (2.28.1)
    Requirement already satisfied: soupsieve>1.2 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from beautifulsoup4->snscrape) (2.3.1)
    Requirement already satisfied: charset-normalizer<3,>=2 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from requests[socks]->snscrape) (2.0.4)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from requests[socks]->snscrape) (1.26.11)
    Requirement already satisfied: certifi>=2017.4.17 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from requests[socks]->snscrape) (2022.9.14)
    Requirement already satisfied: idna<4,>=2.5 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from requests[socks]->snscrape) (3.3)
    Requirement already satisfied: PySocks!=1.5.7,>=1.5.6 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from requests[socks]->snscrape) (1.7.1)
    Installing collected packages: snscrape
    Successfully installed snscrape-0.4.3.20220106
    Note: you may need to restart the kernel to use updated packages.
    


```python
# Import basic libraries
import pandas as pd
import numpy as np
# librerías
import snscrape.modules.twitter as sntwitter
# Load viz library Altair
import altair as alt
#alt.renderers.enable('mimetype') #<-Este nos sirve para ver las gráficas en GitHub
alt.renderers.enable('default') #<-es el más común pero no refleja las gráficas en GitHub
```




    RendererRegistry.enable('default')




```python
%%time

# Parámetros
tweets_list_mx = []
tweets_list_mx1 = []
maxTweets_mx = 10_000

# Obtener tweets #MéxicoDeMiVida
for i,tweet in enumerate(sntwitter.TwitterSearchScraper('#MéxicoDeMiVida').get_items()): # se puede añadir esto --> since:'+date_initial
        if i>maxTweets_mx-1:
            break
        tweets_list_mx.append([tweet.user.username, tweet.date, tweet.id, tweet.content, tweet.url, tweet.lang,
                    tweet.hashtags, tweet.likeCount, tweet.replyCount, tweet.retweetCount, tweet.quoteCount])
  

```

    Wall time: 1min 57s
    


```python
%%time

# Obtener tweets #mexicoargentina
for i,tweet in enumerate(sntwitter.TwitterSearchScraper('#mexicoargentina').get_items()): # se puede añadir esto --> since:'+date_initial
        if i>maxTweets_mx-1:
            break
        tweets_list_mx1.append([tweet.user.username, tweet.date, tweet.id, tweet.content, tweet.url, tweet.lang,
                    tweet.hashtags, tweet.likeCount, tweet.replyCount, tweet.retweetCount, tweet.quoteCount])
  
```

    Wall time: 5min 30s
    


```python
# Columnas
column_names = ("username","date","id","content","url","language","hashtags",
                "likes_count","reply_count","retweet_count","quote_count")
```


```python
# Pandas dataframe con tweets que mencionen el hashtag #MéxicoDeMiVida
df_mx = pd.DataFrame(tweets_list_mx, columns=column_names)
df_mx['date'] = df_mx['date'].dt.strftime('%Y-%m-%d')
df_mx
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>username</th>
      <th>date</th>
      <th>id</th>
      <th>content</th>
      <th>url</th>
      <th>language</th>
      <th>hashtags</th>
      <th>likes_count</th>
      <th>reply_count</th>
      <th>retweet_count</th>
      <th>quote_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CarlosPerez85</td>
      <td>2022-11-30</td>
      <td>1597822811439247360</td>
      <td>Hola Dios, soy yo de nuevo 🙏🏻 \n#MéxicoDeMiVid...</td>
      <td>https://twitter.com/CarlosPerez85/status/15978...</td>
      <td>es</td>
      <td>[MéxicoDeMiVida, MEX]</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>RosaLindaHeide</td>
      <td>2022-11-30</td>
      <td>1597822540566917120</td>
      <td>It’s game day 👊👏🇲🇽👏👊👏🇲🇽🤗🤗🇲🇽👊💪😍👊💪🤗👏🇲🇽\nY con es...</td>
      <td>https://twitter.com/RosaLindaHeide/status/1597...</td>
      <td>es</td>
      <td>[NoMemoNoParty, LaEsperanzaMuereAlÚltimo, Vamo...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>miseleccionmx</td>
      <td>2022-11-30</td>
      <td>1597819964316037120</td>
      <td>Incondicionales, @Bitso quiere saber cuántos t...</td>
      <td>https://twitter.com/miseleccionmx/status/15978...</td>
      <td>es</td>
      <td>[MéxicoDeMiVida, FMFporNuestroFútbol]</td>
      <td>75</td>
      <td>49</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>KirolTV</td>
      <td>2022-11-30</td>
      <td>1597817608929415168</td>
      <td>#Qatar2022 | ¡MEXICO DE MI VIDA!\n\nCon goles ...</td>
      <td>https://twitter.com/KirolTV/status/15978176089...</td>
      <td>es</td>
      <td>[Qatar2022, mex, Lusail, mexicodemivida, EstáE...</td>
      <td>4</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>miseleccionmx</td>
      <td>2022-11-30</td>
      <td>1597816189509582848</td>
      <td>¡Con todo por el triunfo! ⚽️ 🎙\nLas palabras d...</td>
      <td>https://twitter.com/miseleccionmx/status/15978...</td>
      <td>es</td>
      <td>[MéxicoDeMiVida, CelebrandoALaAfición]</td>
      <td>95</td>
      <td>64</td>
      <td>3</td>
      <td>10</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4227</th>
      <td>matidiscenza</td>
      <td>2012-09-27</td>
      <td>251428723964932096</td>
      <td>Dalee verde hoy lo unico q te pido es una aleg...</td>
      <td>https://twitter.com/matidiscenza/status/251428...</td>
      <td>es</td>
      <td>[MexicoDeMiVida]</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4228</th>
      <td>ipinazarignacio</td>
      <td>2012-09-16</td>
      <td>247450056842686464</td>
      <td>Volver a jugar después de 2 meses, ganar y hac...</td>
      <td>https://twitter.com/ipinazarignacio/status/247...</td>
      <td>es</td>
      <td>[MexicoDeMiVida]</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4229</th>
      <td>matidiscenza</td>
      <td>2012-09-16</td>
      <td>247376578798043137</td>
      <td>Dame una alegria hoy  #MexicoDeMiVida daleee</td>
      <td>https://twitter.com/matidiscenza/status/247376...</td>
      <td>es</td>
      <td>[MexicoDeMiVida]</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4230</th>
      <td>_Ara_Morales</td>
      <td>2012-08-11</td>
      <td>234328314662227969</td>
      <td>Waaa ! Como me gustaría estar en las tribunas ...</td>
      <td>https://twitter.com/_Ara_Morales/status/234328...</td>
      <td>es</td>
      <td>[MéxicoDeMiVida]</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4231</th>
      <td>Aaronserfaty</td>
      <td>2011-07-27</td>
      <td>96268168661712896</td>
      <td>@RichardDees @_lucho10 Minuto 51 y estos tipos...</td>
      <td>https://twitter.com/Aaronserfaty/status/962681...</td>
      <td>es</td>
      <td>[mexicoDeMiVida]</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>4232 rows × 11 columns</p>
</div>




```python
# Pandas dataframe con tweets que mencionen el hashtag #mexicoargentina
df_ma = pd.DataFrame(tweets_list_mx1, columns=column_names)
df_ma['date'] = df_ma['date'].dt.strftime('%Y-%m-%d')
df_ma
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>username</th>
      <th>date</th>
      <th>id</th>
      <th>content</th>
      <th>url</th>
      <th>language</th>
      <th>hashtags</th>
      <th>likes_count</th>
      <th>reply_count</th>
      <th>retweet_count</th>
      <th>quote_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>cheloazulay</td>
      <td>2022-11-30</td>
      <td>1597779905059909632</td>
      <td>ATENCION! Canelo usa la bandera de Italia resf...</td>
      <td>https://twitter.com/cheloazulay/status/1597779...</td>
      <td>es</td>
      <td>[Italia, Salame, Canelo, caneloalvarez, canelo...</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Don_opao</td>
      <td>2022-11-30</td>
      <td>1597753080824860672</td>
      <td>Es un puto juego de 11 tipos en calzoncillos c...</td>
      <td>https://twitter.com/Don_opao/status/1597753080...</td>
      <td>es</td>
      <td>[mexicoargentina]</td>
      <td>4</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>keilaarp</td>
      <td>2022-11-30</td>
      <td>1597742730498969600</td>
      <td>#boanoite meus queridos #followers #mexicoarge...</td>
      <td>https://twitter.com/keilaarp/status/1597742730...</td>
      <td>pt</td>
      <td>[boanoite, followers, mexicoargentina, Keilaarp]</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>nisha_roi</td>
      <td>2022-11-29</td>
      <td>1597729367488020481</td>
      <td>There’s always opportunity in real estate. It’...</td>
      <td>https://twitter.com/nisha_roi/status/159772936...</td>
      <td>en</td>
      <td>[realestatequote, credit, realestateagent, inv...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>OlichTmg</td>
      <td>2022-11-29</td>
      <td>1597709784597549056</td>
      <td>Temukan Termurah Good qulity STB Set top box D...</td>
      <td>https://twitter.com/OlichTmg/status/1597709784...</td>
      <td>in</td>
      <td>[ShopeeID, HarryEnColombia, mexicoargentina]</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>9995</th>
      <td>Rorro31159075</td>
      <td>2022-11-26</td>
      <td>1596611009795731458</td>
      <td>Como no nos dejaron 5-0 para mi gano México  v...</td>
      <td>https://twitter.com/Rorro31159075/status/15966...</td>
      <td>es</td>
      <td>[mexicoargentina, MexicoVsArgentina, Messico, ...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9996</th>
      <td>sudhan_zada</td>
      <td>2022-11-26</td>
      <td>1596611009246109697</td>
      <td>This was old Scaloni way sit deep absorb the p...</td>
      <td>https://twitter.com/sudhan_zada/status/1596611...</td>
      <td>en</td>
      <td>[Messi𓃵, FIFAWorldCup, mexicoargentina]</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9997</th>
      <td>tello_raw</td>
      <td>2022-11-26</td>
      <td>1596611007136374784</td>
      <td>Chau boluditos #mexicoargentina #mex https://t...</td>
      <td>https://twitter.com/tello_raw/status/159661100...</td>
      <td>es</td>
      <td>[mexicoargentina, mex]</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9998</th>
      <td>THEEPUNTER</td>
      <td>2022-11-26</td>
      <td>1596611006385778689</td>
      <td>This is a beauty From the magic man himself 🔥 ...</td>
      <td>https://twitter.com/THEEPUNTER/status/15966110...</td>
      <td>en</td>
      <td>[Messi𓃵, mexicoargentina, FIFAWorldCup]</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9999</th>
      <td>fsj965</td>
      <td>2022-11-26</td>
      <td>1596611003848048641</td>
      <td>G.O.A.T 🐐 \n#FIFAWorldCup #mexicoargentina htt...</td>
      <td>https://twitter.com/fsj965/status/159661100384...</td>
      <td>pt</td>
      <td>[FIFAWorldCup, mexicoargentina]</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>10000 rows × 11 columns</p>
</div>




```python
#Extracción de datos a CSV
df_mx.to_csv('twitter_Mexicodemivida')
df_ma.to_csv('twitter_mexicoargentina')
```


```python
# Tweets por fecha #MéxicoDeMiVida
source = pd.DataFrame(df_mx['date'].value_counts()).reset_index().rename(columns={'index':'fecha', 'date':"tweets"})
source
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>fecha</th>
      <th>tweets</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2022-11-26</td>
      <td>1043</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2022-11-22</td>
      <td>1012</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2022-11-16</td>
      <td>216</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2022-11-21</td>
      <td>180</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2022-11-09</td>
      <td>178</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>60</th>
      <td>2022-09-06</td>
      <td>1</td>
    </tr>
    <tr>
      <th>61</th>
      <td>2022-09-21</td>
      <td>1</td>
    </tr>
    <tr>
      <th>62</th>
      <td>2022-09-24</td>
      <td>1</td>
    </tr>
    <tr>
      <th>63</th>
      <td>2022-10-12</td>
      <td>1</td>
    </tr>
    <tr>
      <th>64</th>
      <td>2011-07-27</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>65 rows × 2 columns</p>
</div>



## Fechas importantes de la Selección Mexicana de Futbol
- 22 de Noviembre del 2022 - Debut mundialista ante Polonia (0-0)
- 16 de Noviembre del 2022 - Juengo amistoso contra Suecia (1-2)
- 21 de Noviembre del 2022 - Víspera del juego de la selección contra Polonia
- 25 de Noviembre del 2022 - Víspera del juego contra Argentina
- 09 de Noviembre del 2022 - Partido amistos contra Irak (4-0)



```python
#Tabla con los 10 Tweets con más comentarios

df_mx1 = df_mx.sort_values(by='likes_count', ascending=False).head(10)
df_mx1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>username</th>
      <th>date</th>
      <th>id</th>
      <th>content</th>
      <th>url</th>
      <th>language</th>
      <th>hashtags</th>
      <th>likes_count</th>
      <th>reply_count</th>
      <th>retweet_count</th>
      <th>quote_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1703</th>
      <td>miseleccionmx</td>
      <td>2022-11-22</td>
      <td>1595118934021505024</td>
      <td>🤷🏻‍♂️ \nCinco Copas @yosoy8a. \n\n#MéxicoDeMiV...</td>
      <td>https://twitter.com/miseleccionmx/status/15951...</td>
      <td>es</td>
      <td>[MéxicoDeMiVida]</td>
      <td>20842</td>
      <td>167</td>
      <td>3225</td>
      <td>219</td>
    </tr>
    <tr>
      <th>3366</th>
      <td>miseleccionmx</td>
      <td>2022-11-14</td>
      <td>1592216182656040960</td>
      <td>¡El momento de 𝐬𝐨𝐧̃𝐚𝐫 es ahora! ✨ 🇲🇽\nLa convo...</td>
      <td>https://twitter.com/miseleccionmx/status/15922...</td>
      <td>es</td>
      <td>[MéxicoDeMiVida, Qatar2022]</td>
      <td>12999</td>
      <td>5455</td>
      <td>2126</td>
      <td>3406</td>
    </tr>
    <tr>
      <th>2761</th>
      <td>miseleccionmx</td>
      <td>2022-11-21</td>
      <td>1594526009893695488</td>
      <td>La pasión que provoca el #MéxicoDeMiVida puede...</td>
      <td>https://twitter.com/miseleccionmx/status/15945...</td>
      <td>es</td>
      <td>[MéxicoDeMiVida]</td>
      <td>9808</td>
      <td>469</td>
      <td>2207</td>
      <td>1682</td>
    </tr>
    <tr>
      <th>3356</th>
      <td>miseleccionmx</td>
      <td>2022-11-14</td>
      <td>1592217487386775552</td>
      <td>Lista de 𝟮𝟲 convocados para jugar la 🏆 del 🌎 e...</td>
      <td>https://twitter.com/miseleccionmx/status/15922...</td>
      <td>es</td>
      <td>[MéxicoDeMiVida, FMFporNuestroFútbol]</td>
      <td>9515</td>
      <td>3449</td>
      <td>1402</td>
      <td>2277</td>
    </tr>
    <tr>
      <th>396</th>
      <td>miseleccionmx</td>
      <td>2022-11-26</td>
      <td>1596609206349545472</td>
      <td>Final del partido en Lusail. ⏱\n\n¡A darle la ...</td>
      <td>https://twitter.com/miseleccionmx/status/15966...</td>
      <td>es</td>
      <td>[MéxicoDeMiVida, Qatar2022]</td>
      <td>7504</td>
      <td>8143</td>
      <td>859</td>
      <td>997</td>
    </tr>
    <tr>
      <th>1761</th>
      <td>miseleccionmx</td>
      <td>2022-11-22</td>
      <td>1595113948503998464</td>
      <td>Sumamos nuestro primer punto en #Qatar2022.\n\...</td>
      <td>https://twitter.com/miseleccionmx/status/15951...</td>
      <td>es</td>
      <td>[Qatar2022, MéxicoDeMiVida]</td>
      <td>6517</td>
      <td>809</td>
      <td>814</td>
      <td>284</td>
    </tr>
    <tr>
      <th>3899</th>
      <td>miseleccionmx</td>
      <td>2022-11-03</td>
      <td>1588248176670412800</td>
      <td>¡SANTIAGOOOOOOL! ⚽️ 🇲🇽\nApenas ingresó de camb...</td>
      <td>https://twitter.com/miseleccionmx/status/15882...</td>
      <td>es</td>
      <td>[ConvocadosMX, MéxicoDeMiVida]</td>
      <td>6428</td>
      <td>434</td>
      <td>309</td>
      <td>80</td>
    </tr>
    <tr>
      <th>3282</th>
      <td>yosoy8a</td>
      <td>2022-11-14</td>
      <td>1592267851527766018</td>
      <td>Un SUEÑO hecho realidad poder estar en mi 5ta ...</td>
      <td>https://twitter.com/yosoy8a/status/15922678515...</td>
      <td>es</td>
      <td>[MexicoDeMiVida, NoMemoNoParty, Qatar2022]</td>
      <td>6408</td>
      <td>1448</td>
      <td>459</td>
      <td>140</td>
    </tr>
    <tr>
      <th>2362</th>
      <td>TigresOficial</td>
      <td>2022-11-22</td>
      <td>1595069661388865536</td>
      <td>#MéxicoDeMiVida https://t.co/kTh7jOfIyS</td>
      <td>https://twitter.com/TigresOficial/status/15950...</td>
      <td>qme</td>
      <td>[MéxicoDeMiVida]</td>
      <td>6347</td>
      <td>40</td>
      <td>628</td>
      <td>21</td>
    </tr>
    <tr>
      <th>2356</th>
      <td>CruzAzul</td>
      <td>2022-11-22</td>
      <td>1595070195520995329</td>
      <td>🇲🇽 @miseleccionmx 🇲🇽\n\n#ConTodoMéxico #México...</td>
      <td>https://twitter.com/CruzAzul/status/1595070195...</td>
      <td>und</td>
      <td>[ConTodoMéxico, MéxicoDeMiVida]</td>
      <td>6109</td>
      <td>52</td>
      <td>596</td>
      <td>41</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Gráfica de barras de top 10 de comentarios con más likes 
alt.Chart(df_mx1).mark_bar().encode(
    x="content",
    y="likes_count"
).properties(
    title="Top 10 de comentarios con más likes"
)
```





<div id="altair-viz-d46a83e4b94d4ea391e24d66cb019c0d"></div>
<script type="text/javascript">
  var VEGA_DEBUG = (typeof VEGA_DEBUG == "undefined") ? {} : VEGA_DEBUG;
  (function(spec, embedOpt){
    let outputDiv = document.currentScript.previousElementSibling;
    if (outputDiv.id !== "altair-viz-d46a83e4b94d4ea391e24d66cb019c0d") {
      outputDiv = document.getElementById("altair-viz-d46a83e4b94d4ea391e24d66cb019c0d");
    }
    const paths = {
      "vega": "https://cdn.jsdelivr.net/npm//vega@5?noext",
      "vega-lib": "https://cdn.jsdelivr.net/npm//vega-lib?noext",
      "vega-lite": "https://cdn.jsdelivr.net/npm//vega-lite@4.17.0?noext",
      "vega-embed": "https://cdn.jsdelivr.net/npm//vega-embed@6?noext",
    };

    function maybeLoadScript(lib, version) {
      var key = `${lib.replace("-", "")}_version`;
      return (VEGA_DEBUG[key] == version) ?
        Promise.resolve(paths[lib]) :
        new Promise(function(resolve, reject) {
          var s = document.createElement('script');
          document.getElementsByTagName("head")[0].appendChild(s);
          s.async = true;
          s.onload = () => {
            VEGA_DEBUG[key] = version;
            return resolve(paths[lib]);
          };
          s.onerror = () => reject(`Error loading script: ${paths[lib]}`);
          s.src = paths[lib];
        });
    }

    function showError(err) {
      outputDiv.innerHTML = `<div class="error" style="color:red;">${err}</div>`;
      throw err;
    }

    function displayChart(vegaEmbed) {
      vegaEmbed(outputDiv, spec, embedOpt)
        .catch(err => showError(`Javascript Error: ${err.message}<br>This usually means there's a typo in your chart specification. See the javascript console for the full traceback.`));
    }

    if(typeof define === "function" && define.amd) {
      requirejs.config({paths});
      require(["vega-embed"], displayChart, err => showError(`Error loading script: ${err.message}`));
    } else {
      maybeLoadScript("vega", "5")
        .then(() => maybeLoadScript("vega-lite", "4.17.0"))
        .then(() => maybeLoadScript("vega-embed", "6"))
        .catch(showError)
        .then(() => displayChart(vegaEmbed));
    }
  })({"config": {"view": {"continuousWidth": 400, "continuousHeight": 300}}, "data": {"name": "data-8ea07a03305317666aeafc5747755ba3"}, "mark": "bar", "encoding": {"x": {"field": "content", "type": "nominal"}, "y": {"field": "likes_count", "type": "quantitative"}}, "title": "Top 10 de comentarios con m\u00e1s likes", "$schema": "https://vega.github.io/schema/vega-lite/v4.17.0.json", "datasets": {"data-8ea07a03305317666aeafc5747755ba3": [{"username": "miseleccionmx", "date": "2022-11-22", "id": 1595118934021505024, "content": "\ud83e\udd37\ud83c\udffb\u200d\u2642\ufe0f \nCinco Copas @yosoy8a. \n\n#M\u00e9xicoDeMiVida https://t.co/aNtBeqgOmO", "url": "https://twitter.com/miseleccionmx/status/1595118934021505024", "language": "es", "hashtags": ["M\u00e9xicoDeMiVida"], "likes_count": 20842, "reply_count": 167, "retweet_count": 3225, "quote_count": 219}, {"username": "miseleccionmx", "date": "2022-11-14", "id": 1592216182656040960, "content": "\u00a1El momento de \ud835\udc2c\ud835\udc28\ud835\udc27\u0303\ud835\udc1a\ud835\udc2b es ahora! \u2728 \ud83c\uddf2\ud83c\uddfd\nLa convocatoria de \ud835\udfee\ud835\udff2 jugadores que dejar\u00e1n el coraz\u00f3n por el #M\u00e9xicoDeMiVida en la Copa del Mundo de #Qatar2022. \u26bd\ufe0f \ud83c\udfc6 \ud83c\udf0e\n\n\u00a1VAAMOOOS! \ud83d\udcaa\ud83c\udffb https://t.co/kCDAkGdEaa", "url": "https://twitter.com/miseleccionmx/status/1592216182656040960", "language": "es", "hashtags": ["M\u00e9xicoDeMiVida", "Qatar2022"], "likes_count": 12999, "reply_count": 5455, "retweet_count": 2126, "quote_count": 3406}, {"username": "miseleccionmx", "date": "2022-11-21", "id": 1594526009893695488, "content": "La pasi\u00f3n que provoca el #M\u00e9xicoDeMiVida puede convertirse en una \ud835\udc89\ud835\udc86\ud835\udc93\ud835\udc86\ud835\udc8f\ud835\udc84\ud835\udc8a\ud835\udc82 que perdurar\u00e1 m\u00e1s all\u00e1 de la vida y de generaci\u00f3n en generaci\u00f3n. \ud83d\udc9a\ud83c\uddf2\ud83c\uddfd https://t.co/EeZKvYEszt", "url": "https://twitter.com/miseleccionmx/status/1594526009893695488", "language": "es", "hashtags": ["M\u00e9xicoDeMiVida"], "likes_count": 9808, "reply_count": 469, "retweet_count": 2207, "quote_count": 1682}, {"username": "miseleccionmx", "date": "2022-11-14", "id": 1592217487386775552, "content": "Lista de \ud835\udfee\ud835\udff2 convocados para jugar la \ud83c\udfc6 del \ud83c\udf0e en Qatar \ud83c\uddf6\ud83c\udde6.  \u26bd\ufe0f \ud83c\uddf2\ud83c\uddfd\n\n\u00a1Todos juntos a so\u00f1ar con #M\u00e9xicoDeMiVida! \u2728 \ud83d\udc9a\n\u27a1\ufe0f https://t.co/MezZHvSnUm \n\n#FMFporNuestroF\u00fatbol https://t.co/gdakcTUm6m", "url": "https://twitter.com/miseleccionmx/status/1592217487386775552", "language": "es", "hashtags": ["M\u00e9xicoDeMiVida", "FMFporNuestroF\u00fatbol"], "likes_count": 9515, "reply_count": 3449, "retweet_count": 1402, "quote_count": 2277}, {"username": "miseleccionmx", "date": "2022-11-26", "id": 1596609206349545472, "content": "Final del partido en Lusail. \u23f1\n\n\u00a1A darle la vuelta y comenzar a pensar en \ud83c\uddf8\ud83c\udde6 para buscar la clasificaci\u00f3n! \u26bd\ufe0f\n\n#M\u00e9xicoDeMiVida | #Qatar2022 https://t.co/d3R7hcz9ws", "url": "https://twitter.com/miseleccionmx/status/1596609206349545472", "language": "es", "hashtags": ["M\u00e9xicoDeMiVida", "Qatar2022"], "likes_count": 7504, "reply_count": 8143, "retweet_count": 859, "quote_count": 997}, {"username": "miseleccionmx", "date": "2022-11-22", "id": 1595113948503998464, "content": "Sumamos nuestro primer punto en #Qatar2022.\n\nAhora a pensar y trabajar en el partido del s\u00e1bado ante Argentina. \u26bd\ufe0f\n\u00a1Dale, M\u00e9xico! \ud83d\udc4a\ud83c\udffb \ud83c\uddf2\ud83c\uddfd\n\n#M\u00e9xicoDeMiVida https://t.co/wmTkskr8LY", "url": "https://twitter.com/miseleccionmx/status/1595113948503998464", "language": "es", "hashtags": ["Qatar2022", "M\u00e9xicoDeMiVida"], "likes_count": 6517, "reply_count": 809, "retweet_count": 814, "quote_count": 284}, {"username": "miseleccionmx", "date": "2022-11-03", "id": 1588248176670412800, "content": "\u00a1SANTIAGOOOOOOL! \u26bd\ufe0f \ud83c\uddf2\ud83c\uddfd\nApenas ingres\u00f3 de cambio y @Santigim9 marc\u00f3 el gol con el que @Feyenoord est\u00e1 derrotando a la Lazio en la Europa League. \ud83c\udfc6\n\n#ConvocadosMX | #M\u00e9xicoDeMiVida https://t.co/pdX89TDMwm", "url": "https://twitter.com/miseleccionmx/status/1588248176670412800", "language": "es", "hashtags": ["ConvocadosMX", "M\u00e9xicoDeMiVida"], "likes_count": 6428, "reply_count": 434, "retweet_count": 309, "quote_count": 80}, {"username": "yosoy8a", "date": "2022-11-14", "id": 1592267851527766018, "content": "Un SUE\u00d1O hecho realidad poder estar en mi 5ta Copa del Mundo , con mucha ganas, ilusi\u00f3n y orgullo de representar a mi pa\u00eds en Qatar y poner el nombre de M\u00c9XICO en alto! Con toda la dedicaci\u00f3n,pasi\u00f3n y esfuerzo!\n\n \u00a1VAMOS M\u00c9XICO! \ud83c\uddf2\ud83c\uddfd\n\n#MexicoDeMiVida #NoMemoNoParty #Qatar2022 https://t.co/ItuFtFuYdI", "url": "https://twitter.com/yosoy8a/status/1592267851527766018", "language": "es", "hashtags": ["MexicoDeMiVida", "NoMemoNoParty", "Qatar2022"], "likes_count": 6408, "reply_count": 1448, "retweet_count": 459, "quote_count": 140}, {"username": "TigresOficial", "date": "2022-11-22", "id": 1595069661388865536, "content": "#M\u00e9xicoDeMiVida https://t.co/kTh7jOfIyS", "url": "https://twitter.com/TigresOficial/status/1595069661388865536", "language": "qme", "hashtags": ["M\u00e9xicoDeMiVida"], "likes_count": 6347, "reply_count": 40, "retweet_count": 628, "quote_count": 21}, {"username": "CruzAzul", "date": "2022-11-22", "id": 1595070195520995329, "content": "\ud83c\uddf2\ud83c\uddfd @miseleccionmx \ud83c\uddf2\ud83c\uddfd\n\n#ConTodoM\u00e9xico #M\u00e9xicoDeMiVida https://t.co/DHR8HRHnkQ", "url": "https://twitter.com/CruzAzul/status/1595070195520995329", "language": "und", "hashtags": ["ConTodoM\u00e9xico", "M\u00e9xicoDeMiVida"], "likes_count": 6109, "reply_count": 52, "retweet_count": 596, "quote_count": 41}]}}, {"mode": "vega-lite"});
</script>




```python
#Agrupamos los tweets por usuario y fecha 

mxn_tw = df_mx.groupby(['username','date']).agg(count=('id','count')).reset_index().sort_values(by='date')
mxn_tw
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>username</th>
      <th>date</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>36</th>
      <td>Aaronserfaty</td>
      <td>2011-07-27</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1270</th>
      <td>_Ara_Morales</td>
      <td>2012-08-11</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1633</th>
      <td>ipinazarignacio</td>
      <td>2012-09-16</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1763</th>
      <td>matidiscenza</td>
      <td>2012-09-16</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1764</th>
      <td>matidiscenza</td>
      <td>2012-09-27</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1695</th>
      <td>kingcasteelsG</td>
      <td>2022-11-30</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2033</th>
      <td>terry03692</td>
      <td>2022-11-30</td>
      <td>2</td>
    </tr>
    <tr>
      <th>176</th>
      <td>CITNOVA</td>
      <td>2022-11-30</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1651</th>
      <td>izziprensa</td>
      <td>2022-11-30</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1391</th>
      <td>banani_ap</td>
      <td>2022-11-30</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>2110 rows × 3 columns</p>
</div>




```python
# Gráfica de los tweets que ha publicado el usuario miseleccionmx
alt.Chart(mxn_tw[mxn_tw['username']=='miseleccionmx']).mark_line().encode(
    alt.X("date",title="Fecha"),
    alt.Y('count', title="Frecuencia")
).properties(
    width=800,height=300,
    title="Tweets realizados por el usuario miseleccionmx con el hashtag MexicoDeMiVida"
)
```





<div id="altair-viz-7498cf4c21554a0ca6673104bfcf5866"></div>
<script type="text/javascript">
  var VEGA_DEBUG = (typeof VEGA_DEBUG == "undefined") ? {} : VEGA_DEBUG;
  (function(spec, embedOpt){
    let outputDiv = document.currentScript.previousElementSibling;
    if (outputDiv.id !== "altair-viz-7498cf4c21554a0ca6673104bfcf5866") {
      outputDiv = document.getElementById("altair-viz-7498cf4c21554a0ca6673104bfcf5866");
    }
    const paths = {
      "vega": "https://cdn.jsdelivr.net/npm//vega@5?noext",
      "vega-lib": "https://cdn.jsdelivr.net/npm//vega-lib?noext",
      "vega-lite": "https://cdn.jsdelivr.net/npm//vega-lite@4.17.0?noext",
      "vega-embed": "https://cdn.jsdelivr.net/npm//vega-embed@6?noext",
    };

    function maybeLoadScript(lib, version) {
      var key = `${lib.replace("-", "")}_version`;
      return (VEGA_DEBUG[key] == version) ?
        Promise.resolve(paths[lib]) :
        new Promise(function(resolve, reject) {
          var s = document.createElement('script');
          document.getElementsByTagName("head")[0].appendChild(s);
          s.async = true;
          s.onload = () => {
            VEGA_DEBUG[key] = version;
            return resolve(paths[lib]);
          };
          s.onerror = () => reject(`Error loading script: ${paths[lib]}`);
          s.src = paths[lib];
        });
    }

    function showError(err) {
      outputDiv.innerHTML = `<div class="error" style="color:red;">${err}</div>`;
      throw err;
    }

    function displayChart(vegaEmbed) {
      vegaEmbed(outputDiv, spec, embedOpt)
        .catch(err => showError(`Javascript Error: ${err.message}<br>This usually means there's a typo in your chart specification. See the javascript console for the full traceback.`));
    }

    if(typeof define === "function" && define.amd) {
      requirejs.config({paths});
      require(["vega-embed"], displayChart, err => showError(`Error loading script: ${err.message}`));
    } else {
      maybeLoadScript("vega", "5")
        .then(() => maybeLoadScript("vega-lite", "4.17.0"))
        .then(() => maybeLoadScript("vega-embed", "6"))
        .catch(showError)
        .then(() => displayChart(vegaEmbed));
    }
  })({"config": {"view": {"continuousWidth": 400, "continuousHeight": 300}}, "data": {"name": "data-37aebb24c60ac8f6cd774cd265c9c574"}, "mark": "line", "encoding": {"x": {"field": "date", "title": "Fecha", "type": "nominal"}, "y": {"field": "count", "title": "Frecuencia", "type": "quantitative"}}, "height": 300, "title": "Tweets realizados por el usuario miseleccionmx con el hashtag MexicoDeMiVida", "width": 800, "$schema": "https://vega.github.io/schema/vega-lite/v4.17.0.json", "datasets": {"data-37aebb24c60ac8f6cd774cd265c9c574": [{"username": "miseleccionmx", "date": "2022-09-05", "count": 1}, {"username": "miseleccionmx", "date": "2022-09-08", "count": 1}, {"username": "miseleccionmx", "date": "2022-09-12", "count": 1}, {"username": "miseleccionmx", "date": "2022-09-16", "count": 1}, {"username": "miseleccionmx", "date": "2022-10-26", "count": 21}, {"username": "miseleccionmx", "date": "2022-10-27", "count": 13}, {"username": "miseleccionmx", "date": "2022-10-28", "count": 10}, {"username": "miseleccionmx", "date": "2022-10-29", "count": 10}, {"username": "miseleccionmx", "date": "2022-10-30", "count": 9}, {"username": "miseleccionmx", "date": "2022-10-31", "count": 8}, {"username": "miseleccionmx", "date": "2022-11-01", "count": 16}, {"username": "miseleccionmx", "date": "2022-11-02", "count": 14}, {"username": "miseleccionmx", "date": "2022-11-03", "count": 12}, {"username": "miseleccionmx", "date": "2022-11-04", "count": 7}, {"username": "miseleccionmx", "date": "2022-11-05", "count": 8}, {"username": "miseleccionmx", "date": "2022-11-06", "count": 9}, {"username": "miseleccionmx", "date": "2022-11-07", "count": 8}, {"username": "miseleccionmx", "date": "2022-11-08", "count": 20}, {"username": "miseleccionmx", "date": "2022-11-09", "count": 52}, {"username": "miseleccionmx", "date": "2022-11-10", "count": 11}, {"username": "miseleccionmx", "date": "2022-11-11", "count": 10}, {"username": "miseleccionmx", "date": "2022-11-12", "count": 11}, {"username": "miseleccionmx", "date": "2022-11-13", "count": 15}, {"username": "miseleccionmx", "date": "2022-11-14", "count": 8}, {"username": "miseleccionmx", "date": "2022-11-15", "count": 16}, {"username": "miseleccionmx", "date": "2022-11-16", "count": 53}, {"username": "miseleccionmx", "date": "2022-11-17", "count": 7}, {"username": "miseleccionmx", "date": "2022-11-18", "count": 16}, {"username": "miseleccionmx", "date": "2022-11-19", "count": 12}, {"username": "miseleccionmx", "date": "2022-11-20", "count": 8}, {"username": "miseleccionmx", "date": "2022-11-21", "count": 30}, {"username": "miseleccionmx", "date": "2022-11-22", "count": 61}, {"username": "miseleccionmx", "date": "2022-11-23", "count": 14}, {"username": "miseleccionmx", "date": "2022-11-24", "count": 10}, {"username": "miseleccionmx", "date": "2022-11-25", "count": 33}, {"username": "miseleccionmx", "date": "2022-11-26", "count": 46}, {"username": "miseleccionmx", "date": "2022-11-27", "count": 3}, {"username": "miseleccionmx", "date": "2022-11-28", "count": 10}, {"username": "miseleccionmx", "date": "2022-11-29", "count": 20}, {"username": "miseleccionmx", "date": "2022-11-30", "count": 8}]}}, {"mode": "vega-lite"});
</script>




```python
#Tweets agrupados por idioma

mxn_ln = df_mx.groupby(['language']).agg(count=('id','count')).reset_index().sort_values(by='count',ascending=False).head(10)
mxn_ln
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>language</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6</th>
      <td>es</td>
      <td>3060</td>
    </tr>
    <tr>
      <th>5</th>
      <td>en</td>
      <td>720</td>
    </tr>
    <tr>
      <th>28</th>
      <td>und</td>
      <td>179</td>
    </tr>
    <tr>
      <th>22</th>
      <td>qme</td>
      <td>63</td>
    </tr>
    <tr>
      <th>21</th>
      <td>qht</td>
      <td>32</td>
    </tr>
    <tr>
      <th>20</th>
      <td>pt</td>
      <td>30</td>
    </tr>
    <tr>
      <th>0</th>
      <td>ar</td>
      <td>26</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ca</td>
      <td>14</td>
    </tr>
    <tr>
      <th>19</th>
      <td>pl</td>
      <td>13</td>
    </tr>
    <tr>
      <th>13</th>
      <td>in</td>
      <td>12</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gráfica de los idiomas de los tweets en #MexicoDeMiVida

alt.Chart(mxn_ln).mark_arc().encode(
    theta=alt.Theta(field="count", type="quantitative"),
    color=alt.Color(field="language", type="nominal"),
).properties(
    title="Idioma de los Tweets #MexicoDeMiVida"
)
```





<div id="altair-viz-63a858f00e134690a7275a4421833513"></div>
<script type="text/javascript">
  var VEGA_DEBUG = (typeof VEGA_DEBUG == "undefined") ? {} : VEGA_DEBUG;
  (function(spec, embedOpt){
    let outputDiv = document.currentScript.previousElementSibling;
    if (outputDiv.id !== "altair-viz-63a858f00e134690a7275a4421833513") {
      outputDiv = document.getElementById("altair-viz-63a858f00e134690a7275a4421833513");
    }
    const paths = {
      "vega": "https://cdn.jsdelivr.net/npm//vega@5?noext",
      "vega-lib": "https://cdn.jsdelivr.net/npm//vega-lib?noext",
      "vega-lite": "https://cdn.jsdelivr.net/npm//vega-lite@4.17.0?noext",
      "vega-embed": "https://cdn.jsdelivr.net/npm//vega-embed@6?noext",
    };

    function maybeLoadScript(lib, version) {
      var key = `${lib.replace("-", "")}_version`;
      return (VEGA_DEBUG[key] == version) ?
        Promise.resolve(paths[lib]) :
        new Promise(function(resolve, reject) {
          var s = document.createElement('script');
          document.getElementsByTagName("head")[0].appendChild(s);
          s.async = true;
          s.onload = () => {
            VEGA_DEBUG[key] = version;
            return resolve(paths[lib]);
          };
          s.onerror = () => reject(`Error loading script: ${paths[lib]}`);
          s.src = paths[lib];
        });
    }

    function showError(err) {
      outputDiv.innerHTML = `<div class="error" style="color:red;">${err}</div>`;
      throw err;
    }

    function displayChart(vegaEmbed) {
      vegaEmbed(outputDiv, spec, embedOpt)
        .catch(err => showError(`Javascript Error: ${err.message}<br>This usually means there's a typo in your chart specification. See the javascript console for the full traceback.`));
    }

    if(typeof define === "function" && define.amd) {
      requirejs.config({paths});
      require(["vega-embed"], displayChart, err => showError(`Error loading script: ${err.message}`));
    } else {
      maybeLoadScript("vega", "5")
        .then(() => maybeLoadScript("vega-lite", "4.17.0"))
        .then(() => maybeLoadScript("vega-embed", "6"))
        .catch(showError)
        .then(() => displayChart(vegaEmbed));
    }
  })({"config": {"view": {"continuousWidth": 400, "continuousHeight": 300}}, "data": {"name": "data-c100a7ae5cf9f9bcdcaff3baa7ecf85b"}, "mark": "arc", "encoding": {"color": {"field": "language", "type": "nominal"}, "theta": {"field": "count", "type": "quantitative"}}, "title": "Idioma de los Tweets #MexicoDeMiVida", "$schema": "https://vega.github.io/schema/vega-lite/v4.17.0.json", "datasets": {"data-c100a7ae5cf9f9bcdcaff3baa7ecf85b": [{"language": "es", "count": 3060}, {"language": "en", "count": 720}, {"language": "und", "count": 179}, {"language": "qme", "count": 63}, {"language": "qht", "count": 32}, {"language": "pt", "count": 30}, {"language": "ar", "count": 26}, {"language": "ca", "count": 14}, {"language": "pl", "count": 13}, {"language": "in", "count": 12}]}}, {"mode": "vega-lite"});
</script>




```python
#Buscamos el tweet publicado en japones 

df_mx[df_mx['language']=='ja']
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>username</th>
      <th>date</th>
      <th>id</th>
      <th>content</th>
      <th>url</th>
      <th>language</th>
      <th>hashtags</th>
      <th>likes_count</th>
      <th>reply_count</th>
      <th>retweet_count</th>
      <th>quote_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1538</th>
      <td>JerseumStore</td>
      <td>2022-11-23</td>
      <td>1595311184902705153</td>
      <td>【2014】 / Mexico（H） \n\n【オンライン限定商品】\n※店頭販売は行ってお...</td>
      <td>https://twitter.com/JerseumStore/status/159531...</td>
      <td>ja</td>
      <td>[MéxicoDeMiVida, FIFAWorldCup, Qatar2022]</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
pip install spacy
```

    Requirement already satisfied: spacy in c:\users\rosa.guerrero\anaconda3\lib\site-packages (3.4.3)
    Requirement already satisfied: srsly<3.0.0,>=2.4.3 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (2.4.5)
    Requirement already satisfied: spacy-loggers<2.0.0,>=1.0.0 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (1.0.3)
    Requirement already satisfied: murmurhash<1.1.0,>=0.28.0 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (1.0.9)
    Requirement already satisfied: pathy>=0.3.5 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (0.10.0)
    Requirement already satisfied: tqdm<5.0.0,>=4.38.0 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (4.64.1)
    Requirement already satisfied: requests<3.0.0,>=2.13.0 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (2.28.1)
    Requirement already satisfied: packaging>=20.0 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (21.3)
    Requirement already satisfied: pydantic!=1.8,!=1.8.1,<1.11.0,>=1.7.4 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (1.10.2)
    Requirement already satisfied: preshed<3.1.0,>=3.0.2 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (3.0.8)
    Requirement already satisfied: numpy>=1.15.0 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (1.21.5)
    Requirement already satisfied: catalogue<2.1.0,>=2.0.6 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (2.0.8)
    Requirement already satisfied: cymem<2.1.0,>=2.0.2 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (2.0.7)
    Requirement already satisfied: spacy-legacy<3.1.0,>=3.0.10 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (3.0.10)
    Requirement already satisfied: setuptools in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (65.6.3)
    Requirement already satisfied: typer<0.8.0,>=0.3.0 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (0.7.0)
    Requirement already satisfied: jinja2 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (2.11.3)
    Requirement already satisfied: thinc<8.2.0,>=8.1.0 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (8.1.5)
    Requirement already satisfied: wasabi<1.1.0,>=0.9.1 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (0.10.1)
    Requirement already satisfied: langcodes<4.0.0,>=3.2.0 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from spacy) (3.3.0)
    Requirement already satisfied: pyparsing!=3.0.5,>=2.0.2 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from packaging>=20.0->spacy) (3.0.9)
    Requirement already satisfied: smart-open<6.0.0,>=5.2.1 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from pathy>=0.3.5->spacy) (5.2.1)
    Requirement already satisfied: typing-extensions>=4.1.0 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from pydantic!=1.8,!=1.8.1,<1.11.0,>=1.7.4->spacy) (4.3.0)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from requests<3.0.0,>=2.13.0->spacy) (1.26.11)
    Requirement already satisfied: certifi>=2017.4.17 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from requests<3.0.0,>=2.13.0->spacy) (2022.9.14)
    Requirement already satisfied: charset-normalizer<3,>=2 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from requests<3.0.0,>=2.13.0->spacy) (2.0.4)
    Requirement already satisfied: idna<4,>=2.5 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from requests<3.0.0,>=2.13.0->spacy) (3.3)
    Requirement already satisfied: blis<0.8.0,>=0.7.8 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from thinc<8.2.0,>=8.1.0->spacy) (0.7.9)
    Requirement already satisfied: confection<1.0.0,>=0.0.1 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from thinc<8.2.0,>=8.1.0->spacy) (0.0.3)
    Requirement already satisfied: colorama in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from tqdm<5.0.0,>=4.38.0->spacy) (0.4.5)
    Requirement already satisfied: click<9.0.0,>=7.1.1 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from typer<0.8.0,>=0.3.0->spacy) (8.0.4)
    Requirement already satisfied: MarkupSafe>=0.23 in c:\users\rosa.guerrero\anaconda3\lib\site-packages (from jinja2->spacy) (2.0.1)
    Note: you may need to restart the kernel to use updated packages.
    


```python
## librería spaCy
import spacy

```


```python
nlp = spacy.load("es_core_news_sm")
```


```python
from spacytextblob.spacytextblob import SpacyTextBlob
```


    ---------------------------------------------------------------------------

    ModuleNotFoundError                       Traceback (most recent call last)

    ~\AppData\Local\Temp\ipykernel_13728\666843081.py in <module>
    ----> 1 from spacytextblob.spacytextblob import SpacyTextBlob
    

    ModuleNotFoundError: No module named 'spacytextblob'



```python

```
