# laboratorio-5
laboratorio 5

[![imagen-2025-04-30-214740195.png](https://i.postimg.cc/4335gPHs/imagen-2025-04-30-214740195.png)](https://postimg.cc/75p71gNW)

# Descripción del Experimento
El experimento se enfoca en analizar la variabilidad de la frecuencia cardíaca (HRV) utilizando la Transformada Wavelet para identificar cambios en las frecuencias características y estudiar la dinámica temporal de la señal cardíaca. Esto permite evaluar la actividad del sistema nervioso autónomo (simpático y parasimpático) a través de las fluctuaciones en los intervalos R-R del electrocardiograma (ECG). El análisis se realiza en dos dominios:

Dominio del tiempo : Cálculo de parámetros como la media y desviación estándar de los intervalos R-R.

Dominio tiempo-frecuencia : Aplicación de la Transformada Wavelet para observar variaciones espectrales en bandas de baja (LF: 0.04–0.15 Hz) y alta frecuencia (HF: 0.15–0.4 Hz), asociadas a la regulación autonómica.

# Materiales y Equipos
*Laboratorio*

- Software :
  
Python (con bibliotecas como PyWavelets para análisis wavelet).

- Herramientas para diseño de filtros digitales
  
Documentación : Guías teóricas sobre HRV y Transformada Wavelet.

- Estudiante:
  
1)Electrodos de superficie (3 unidades) para captura de ECG.
2)Sistema de adquisición de datos (DAQ, Arduino, STM32 u otros).
3)Cables de conexión para electrodos y sistema DAQ.

# Objetivo

Analizar la variabilidad de la frecuencia cardíaca (HRV) utilizando la Transformada Wavelet para identificar cambios en las frecuencias características y estudiar la dinámica temporal de la señal cardíaca, con el fin de evaluar la actividad del sistema nervioso autónomo (simpático y parasimpático).

# Resultados esperados 

El experimento debe demostrar que la Transformada Wavelet ofrece una ventaja sobre el análisis tradicional en el dominio del tiempo, al capturar variaciones dinámicas en la HRV que reflejan la interacción entre los sistemas simpático y parasimpático. 

# Estructura del Experimento

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

