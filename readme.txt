PROYECTO: Prueba Ejercicios Performance
API de Evaluación: FakeStoreAPI
Endpoint de Evaluación: POST /auth/login
Herramienta: Apache JMeter 5.6.3
Tester: Luis
Fecha: 13/02/2026

Nota: Se adjunto el reporte HTML para su visualizacion, dentro del archivo se encontraran los comandos para la creacion del mismo.

1- Objetivo

Evaluar el comportamiento del endpoint de autenticación bajo condiciones de carga controlada, validando:

- correcto funcionamiento funcional del servicio
- tiempo de respuesta
- estabilidad bajo carga constante
- tasa de errores
- cumplimiento de SLA (TPS, tiempo de respuesta y tasa de error)


2- Tipo de prueba

Prueba de Carga

Simulación de carga constante de 20 requests por segundo (20 TPS) durante alrededor de 60 segundos.


3- Configuración de la prueba

Thread Group:

	- Number of Threads :	30
	- Ramp - up period: 	5
	- Loop Count:		Infinite (Check)
	- Duration:  		60

NOTA: Se configuraron 30 (Number of Threads) para obtener suficientes hilos activos que permitan sostener los 20 TPS, los 5 (Ramp - up period) se utilizaron para simular el consumo progresivo de los usuarios y 60 (Duration) con la finalidad de que la prueba (no sea infinita y se ejecute por esa cantidad de segundos)

CSV Data Set Config:

	- Archivo CSV con usuarios y contraseñas
	- Variables utilizadas: ${user}, ${passwd}
	- Filename :		./Data/input_data.csv
	- File Encoding:	UTF-8
	- Delimiter:		,
	- (Predeterminado)

HTTP Header Manager

	- Content-Type: application/json

Constant Throughput Timer
	- Target Throughput: 1200

Nota: Se utilizo para regular el ritmo de ejecución para mantener los 20 TPS

HTTP Request
	- POST
	- Protocol:	https
	- Server:	fakestoreapi.com
	- Path:		/auth/login
	- Body : 
			{
			  "username": "${user}",
			  "password": "${passwd}"
			}
Assertions:

	- Response Assertion
		- Apply ti: 		Main sample only
		- Field to test:	Test Response
		- Patterns to test:	token

	- Duration Assertion
		- Duration on milliseconds: 1500

Generación de reportes
	Para generar el reporte:
		.\jmeter.bat -n -t archivo.jmx -l resultados.jtl -e -o reporte_html
	Ejemplo en mi equipo (tengo Jmeter portable)
		C:\JMeter\apache-jmeter-5.6.3\bin\jmeter.bat -n -t Proyecto_Login_20TPS.jmx -l resultados.jtl -e -o reporte_html




5- Estructura de entrega

Carpetas: 

/Proyecto_Jmeter_FakeStoreApi_Login/
│
├── Data
├── HTTP Header Manager.jmx
├── readme.txt
└── conclusiones.txt

Estructura proyecto Jmeter

/Test Plan - FakeStoreAPI - Login Load Test /
├── Thread Group - Login_20TPS_60s
	├── CSV Data Set Config - CSV_User_Data
	├── HTTP Header Manager - HTTP_Header
	├── Constant Throughput Timer - Constant_Throughput_Timer_20TPS
	├── HTTP Request - Login_Request_Post
		├── Response Assertion - Assert_Response_Contains_Token
		├── Duration Assertion - Assert_Response_Time<=1500ms
	├── Listener - Listener_View_Results_Tree
	├── Listener - Listener_Summary_Report
