# laboratorio-5
laboratorio 5

[![imagen-2025-04-30-214740195.png](https://i.postimg.cc/4335gPHs/imagen-2025-04-30-214740195.png)](https://postimg.cc/75p71gNW)

# Descripción del Experimento
El experimento se enfoca en analizar la variabilidad de la frecuencia cardíaca (HRV) utilizando la Transformada Wavelet para identificar cambios en las frecuencias características y estudiar la dinámica temporal de la señal cardíaca. Esto permite evaluar la actividad del sistema nervioso autónomo (simpático y parasimpático) a través de las fluctuaciones en los intervalos R-R del electrocardiograma (ECG). El análisis se realiza en dos dominios:

Dominio del tiempo : Cálculo de parámetros como la media y desviación estándar de los intervalos R-R.

Dominio tiempo-frecuencia : Aplicación de la Transformada Wavelet para observar variaciones espectrales en bandas de baja (LF: 0.04–0.15 Hz) y alta frecuencia (HF: 0.15–0.4 Hz), asociadas a la regulación autonómica.

# MATERIAL Y EQUIPOS
*Laboratorio*

- Software :
  
Python (con bibliotecas como PyWavelets para análisis wavelet).

- Herramientas para diseño de filtros digitales
  
Documentación : Guías teóricas sobre HRV y Transformada Wavelet.

- Estudiante:
  
1)Electrodos de superficie (3 unidades) para captura de ECG.
2)Sistema de adquisición de datos (DAQ, Arduino, STM32 u otros).
3)Cables de conexión para electrodos y sistema DAQ.

# OBJETIVO

Analizar la variabilidad de la frecuencia cardíaca (HRV) utilizando la Transformada Wavelet para identificar cambios en las frecuencias características y estudiar la dinámica temporal de la señal cardíaca, con el fin de evaluar la actividad del sistema nervioso autónomo (simpático y parasimpático).

# RESULTADOS ESPERADOS

El experimento debe demostrar que la Transformada Wavelet ofrece una ventaja sobre el análisis tradicional en el dominio del tiempo, al capturar variaciones dinámicas en la HRV que reflejan la interacción entre los sistemas simpático y parasimpático. 

# ESTRUCUTURA DEL EXPERIMENTO 

*1)Fundamento Teórico* 

- Sistema nervioso autónomo (SNA) :
  
Sistema simpático : Responsable de la respuesta "lucha o huida" (aumenta frecuencia cardíaca).

Sistema parasimpático : Promueve la relajación ("descanso y digestión").

- Variabilidad de la frecuencia cardíaca (HRV) :
  
Se mide mediante fluctuaciones en los intervalos R-R del ECG.

Parámetros en dominio del tiempo : Media, SDNN (desviación estándar), RMSSD.

- Bandas frecuenciales :
  
LF (0.04–0.15 Hz) : Relacionada con actividad simpática.

HF (0.15–0.4 Hz) : Relacionada con actividad parasimpática.

- Transformada Wavelet :
  
La Transformada Wavelet es una técnica matemática utilizada para el análisis de señales y datos. A diferencia de la Transformada de Fourier, que proporciona información sobre la frecuencia, la Transformada Wavelet permite una representación tanto en el dominio del tiempo como en el de la frecuencia, lo que la hace especialmente útil para señales no estacionarias.

Análisis tiempo-frecuencia para señales no estacionarias.


*2)Adquisición de la señal ECG*
se escoge al paciente de prueba y que este mismo este en condiciones de reposo como ejemplo acostado o sentado pero debe estar en un estado calmado ,se debe Limpiar la zona donde se colocarán los electrodos para reducir la resistencia de la piel y mejorar la conductividad esto con ayuda de la gel para los electrodos.

[![imagen-2025-05-01-154555335.png](https://i.postimg.cc/NjKTyBZW/imagen-2025-05-01-154555335.png)](https://postimg.cc/JsWtvVrq)

Se deben colocar los electrodos como se muestra en la imagen anterior, despues de colocar los electrodos el sujeto debe  permanezca inmóvil durante la grabación para evitar artefactos por movimiento.
Ya con la preparacion atencian lo siguiente en resvisar es el sistema de adquisición (DAQ)concentado el módulo de captura AD8232  al sistema DAQ y Verificar que la frecuencia de muestreo sea ≥ 250 Hz.

# PROCEDIMINETO 

# *1)calculos del filtro*

El filtro que fue usado es un filtro digital de tipo Butterworth pasa bajos. Este tipo de filtro se selecciono por su respuesta suave en frecuencia y porque no introduce ondulaciones en la banda pasante, lo cual es ideal para preservar la morfología de la señal cardiaca.

 La elección de una frecuencia de corte de 45 Hz nos ayuda  a la necesidad de eliminar componentes de alta frecuencia, como el ruido muscular o interferencias electromagnéticas, sin afectar las componentes útiles del ECG,

El orden 5 del filtro representa una buena capacidad de atenuación fuera de la banda de interés y la estabilidad computacional del sistema, ya que órdenes más altos podrían generar inestabilidades o distorsiones. Además, este diseño responde directamente a los objetivos  del laboratorio.

el codigo que se uso para esto fue el siguiente:

    from scipy.signal import butter, lfilter

     def butter_lowpass(cutoff, fs, order=5):
    nyquist = 0.5 * fs
    normal_cutoff = cutoff / nyquist
    b, a = butter(order, normal_cutoff, btype='low', analog=False)

    print("\n--- Ecuación en diferencias del filtro IIR (Butterworth) ---")
    print("Coeficientes b:", b)
    print("Coeficientes a:", a)
    print("Forma general: y[n] = Σ(b_i * x[n - i]) - Σ(a_j * y[n - j])")

    return b, a

    def butter_lowpass_filter(data, cutoff, fs, order=5):
    b, a = butter_lowpass(cutoff, fs, order)
    y = lfilter(b, a, data)
    return y

 El diseño del filtro se realiza en la función butter_lowpass, donde se calcula primero la frecuencia de Nyquist (la mitad de la frecuencia de muestreo) y se usa para normalizar la frecuencia de corte deseada tambien 
muestra los coeficientes del filtro, que se usarán en la ecuación usando la ecuacion 
y[n]=b 0 x[n]+b 1 x[n−1]+⋯−a 1 y[n−1]−a 2 y[n−2]+⋯

- los parametros del filtro son:
  
  Orden del filtro (order): 5

  Frecuencia de muestreo (fs): 250 Hz

  Frecuencia de corte (cutoff): 45 Hz

  Frecuencia de Nyquist: 125 Hz

# *2)Cálculo de la Frecuencia*

La frecuencia de corte de 45 Hz fue elegida basada en criterios fisiológicos ya que Es lo suficientemente alto para conservar toda la información fisiológica relevante (especialmente el QRS, necesario para detectar picos R)
 Pero lo suficientemente bajo para eliminar ruido no deseado de alta frecuenciay prácticos para preservar el ECG y eliminar ruido. Luego se normalizó dividiéndola entre la frecuencia de Nyquist (125 Hz) para usarla en el diseño digital del filtro. Así se obtuvo una frecuencia normalizada de 0.36, que es lo que el filtro realmente usa internamente.

frecuencia normalizada= frecuencia de corte/frecuencia de Nyquist

donde: 

frecuencia de corte=45 Hz

frecuencia de Nyquist= fs/2​ = 250/2 =125 Hz

Frecuencia normalizada= 45/125 =0.36

# *3)Detección de Picos R*

    picos, _ = find_peaks(señal_filtrada, height=np.max(señal_filtrada)*0.5, distance=0.4*fs)

Los picos R se detectan con find_peaks() aplicada sobre la señal ECG filtrada. Se ajusta un umbral de altura y una distancia mínima entre picos para garantizar que solo se detecten los verdaderos picos R. Esta parte del código es fundamental porque los intervalos entre picos R son la base del análisis de HRV, tanto en el dominio del tiempo como en la transformada wavelet.

# *4)Cálculo de Intervalos R-R*
​
El cálculo de los intervalos R-R se hace tomando la diferencia entre las posiciones de los picos R consecutivos (np.diff(picos)) y dividiendo entre la frecuencia de muestreo (fs) para obtener el resultado en segundos. Estos valores representan el tiempo entre latidos y son fundamentales para calcular los parámetros de variabilidad de la frecuencia cardíaca.

# *5)Desviación estándar de los intervalos R-R: Refleja la dispersión y la variabilidad general de la seña*

    sdnn = np.std(intervalos_rr)


 la línea sdnn = np.std(intervalos_rr) calcula la desviación estándar de todos los intervalos R-R detectados, lo cual representa la variabilidad global de la frecuencia cardíaca del sujeto durante los 5 minutos de grabación. Este valor es uno de los principales indicadores en el dominio del tiempo dentro del análisis de HRV.

 # *6)Transformada Wavelet en Ventanas*

    def calcular_espectrograma_wavelet(signal, fs):
    scales = np.arange(1, 40)
    wavelet = 'cmor1.5-1.0'
    coef, freqs = pywt.cwt(signal, scales, wavelet, sampling_period=1/fs)

    plt.figure(figsize=(12, 6))
    plt.imshow(np.abs(coef), extent=[0, len(signal)/fs, scales.min(), scales.max()],
               cmap='jet', aspect='auto', interpolation='bilinear')
    plt.colorbar(label='Magnitud')
    plt.xlabel("Tiempo (s)")
    plt.ylabel("Escala (relacionada con frecuencia)")
    plt.title("Espectrograma HRV con Transformada Wavelet Continua (Morlet)")
    plt.show()

la transformada wavelet  toma la señal de intervalos R-R, la descompone en múltiples escalas (frecuencias) usando la wavelet Morlet, y genera un espectrograma que muestra cómo varía la energía en diferentes bandas de frecuencia a lo largo del tiempo. Esto permite evaluar dinámicamente la actividad simpática y parasimpática, lo cual es el objetivo principal del laboratorio.

# RESULTADOS 
- *1) grafica de la señal obtenida*
- 
 [![imagen-2025-05-01-181032449.png](https://i.postimg.cc/G3g8Y9Gq/imagen-2025-05-01-181032449.png)](https://postimg.cc/kD8gPJjb)
