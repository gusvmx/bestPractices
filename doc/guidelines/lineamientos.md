# Lineamientos.
## Principios.
### Principio de la única responsabilidad - SRP (Single Responsibility Principle).
El principio de la única responsabilidad indica que una clase o módulo debe tener una, y sólo una, razón para cambiar. Así, este principio nos da la definición de responsabilidad y una guía para el tamaño de la clase. 

Al intentar identificar responsabilidades (razones para cambiar) regularmente ayuda a reconocer y crear mejores abstracciones en nuestro código. Por ejemplo, podemos contar con una clase que maneje componentes Java Swing y al mismo tiempo muestra la versión del sistema. Sin duda queremos cambiar la versión del sistema si tenemos algún cambio en el código Swing, pero esto no es necesariamente cierto. Si cambia algún código de otra sección del sistema, la versión del sistema debe ser cambiada también. 

El SRP es uno de los más importantes conceptos en diseño orientado a objetos. Además es uno de los conceptos más simples de entender y de apegarse a él. Aunque extrañamente, El SRP es con frecuencia el principio de diseño de clases del cual se abusa más. Con regularidad encontramos clases que hacen demasiadas cosas. 

**Para enfatizar**. Queremos que nuestros sistemas estén compuestos por muchas clases pequeñas, no por unas cuantas muy grandes. Cada clase pequeña encapsula una única responsabilidad, tiene una única razón de cambio y colabora con otras pocas para lograr el comportamiento deseado en el sistema.

### Acoplamiento holgado - Loose coupling 
Dos clases, componentes o módulos están acoplados cuando al menos uno de ellos utiliza el otro. Mientras estos elementos sepan menos uno del otro, estos son más holgados y como resultado se tiene un menor impacto al realizar algún cambio.

### Alta cohesión
Cohesión es el grado en que elementos de un todo pertenecen juntos. Métodos y campos en una simple clase, así como las clases de un componente deben de tener una alta cohesión. Una alta cohesión en clases y componentes resulta en un diseño y código más simple y fácil de entender. Cada uno de los métodos de una clase deben manipular una o más de las variables de instancia definidas en la clase. En general mientras más variables manipula un método, el método es más cohesivo a su clase. Una clase en la que cada variable es utilizada por cada uno de los métodos es cohesiva al máximo. En general es casi imposible llegar a esa cohesión máxima; pero por otro lado, nos gustaría mantener una alta cohesión. Cuando la cohesión es alta, significa que los métodos y variables de la clase son codependientes y se mantienen juntos como un todo lógico. Por ejemplo:

```
public class Stack {

	private int topO fStack = 0 ;
	List<Integer> elements = new LinkedList<Integer>();

	public int size() {
		return topOfStack;
	}

	public void push (int element) {
		topOfStack++;
		elements.add(element);
	}

	public int pop() throws PoppedWhenEmpty {
		if (topOfStack == 0)
			throw new PoppedWhenEmpty();
		int element = elements.get(−−topOfStack);
		elements.remove(topOfStack);
		return element;
	}
}

```

La implementación de la pila es una clase muy cohesiva. De los tres métodos, solamente size() no ocupa ambas variables.

Es importante mencionar que al seguir la estrategia de mantener métodos pequeños, así como reducir la cantidad de parámetros, en ocasiones puede resultar en una proliferación de variables que son usadas por un subconjunto de métodos. Cuando esto sucede, casi siempre significa que existe otra clase que intenta salir de la clase grande. 

**Se recomienda** intentar separar las variables y métodos en dos o más clases donde las clases resultantes son más cohesivas.

## Diseño de clases.
### Código a un nivel de abstracción erróneo
Es importante crear abstracciones que separen los conceptos de alto nivel de los conceptos de bajo nivel. En ocasiones podemos lograr esto al crear clases abstractas que contienen los conceptos de alto nivel y las clases concretas que mantienen los conceptos de bajo nivel.

Por ejemplo, constantes, variables o métodos de utilería que pertenecen solamente a la implementación detallada no deben de estar presentes en la clase base. La clase base/abstracta no debe de saber nada de estos detalles.

Esta regla aplica también para archivos fuente, componentes y módulos. Un buen diseño de software requiere que separemos los conceptos en diferentes niveles y colocarlos en diferentes contenedores. A veces estos contenedores son clases base o concretas y en ocasiones en archivos fuente, módulos o componentes.

Considerando el siguiente fragmento de código:

```
public interface Stack {
	Object pop() throws EmptyException;
	void push(Object o) throws FullException;
	double percentFull();
}
```

El método percentFull está a un nivel incorrecto de abstracción. A pesar de que en muchas implementaciones de la pila el concepto de lleno sea razonable, existen otras implementaciones que simplemente no podrían saber que tan llenas están. Por lo tanto el método debería ser colocado en una interfaz hija:

```
public interface BoundedStack extends Stack {
	double percentFull();
}

```

### Demasiada información.
Módulos bien definidos tienen interfaces muy pequeñas que permiten hacer mucho con muy poco. Módulos definidos pobremente tienen interfaces muy amplias y profundas que fuerzan a usar muchas cosas para poder realizar cosas muy simples. Una interfaz bien definida no ofrece muchas funciones o métodos de los que se pueda depender, por lo que el acoplamiento es bajo. Una interfaz pobremente definida provee muchas funciones que debes de llamar, por lo tanto el acoplamiento es alto.

Los buenos desarrolladores de software aprenden a limitar lo que exponen en las interfaces de sus clases y módulos. Mientras menos métodos una clase tenga, mejor. Mientras menos variables conozca una función, mejor. Mientras menos variables de instancia tenga una clase, mejor.

Oculta tu información. Oculta tus funciones de utilería. Oculta tus constantes, variables y métodos temporales. No crees clases con múltiples métodos o cantidades inmensas de variables de instancia. Concéntrate en mantener las interfaces muy justas y pequeñas. Ayuda a mantener el acoplamiento bajo al limitar la cantidad de información.

### Principio abierto-cerrado (Open-closed principle)
El principio Open Closed indica que las clases, módulos y funciones deben estar abiertas para la extensión y cerradas para la modificación. En otras palabras, la clase debes ser capaz de permitir la modificación de su comportamiento sin alterarla.

Este principio se volvió popular al referirse al uso de interfaces, donde las implementaciones pueden ser cambiadas y múltiples implementaciones pudieran ser creadas y polimórficamente sustituidas unas por otras.

### Principio de la inversión de la dependencia
La razón más común para dividir conceptos entre clases base/abstractas y derivadas/concretas es que los conceptos de alto nivel de la clase base pueden ser independientes de los conceptos de bajo nivel de la clase concretas. Entonces, cuando vemos clases base que mencionan nombres de las clases concretas, se sospecha de un problema. En general las clases base no deben saber nada acerca de las concretas.

Existen excepciones a la regla, por supuesto. En ocasiones la cantidad de clases concretas es fijo y la clase base tiene el código para seleccionar entre ellas. Un ejemplo es en implementaciones de máquinas de estado.

**En general** queremos tener la habilidad de poder desplegar en JARs diferentes las clases base y las clases concretas.

### Las clases deben ser pequeñas.
Las clases pequeñas son más fácil de comprender. Estas deben tener menos de 400 líneas de código. De lo contrario, es más difícil de saber cómo ejecuta su tarea y muy probablemente hace más de una sola tarea.

## General.
###  Entender el algoritmo.
Mucho código está sucio y rebuscado porque la gente no se toma el tiempo para entender el algoritmo. Lo hacen funcionar al colocar suficientes sentencias if y banderas sin realmente parase a considerar lo que realmente sucede.

Programar es en ocasiones un proceso exploratorio. Piensas que sabes el algoritmo correcto para algo, pero no es sino hasta que juegas con él y lo haces “funcionar”. ¿Cómo sabes que “funciona”? Porque pasa los casos de prueba que pudiste imaginar.

No hay nada en contra de este enfoque. De hecho, en muchas ocasiones es la única manera de hacer funcionar algo en la manera que debe. Sin embargo, no es suficiente dejar las comillas alrededor de la palabra “funciona”.

Antes de considerar que ya acabaste una función, asegúrate que entendiste como funciona. No es suficiente con que pase las pruebas. Debes de saber que la solución es la correcta.

Comúnmente la mejor manera de obtener este conocimiento y entendimiento es refactorizando la función en algo que es tan limpio y expresivo que resulta obvio saber como funciona.

### Keep it simple
Mientras más sencillo, mejor. Reducir la complejidad ciclomática tanto como sea posible.

### La regla del boy scout.
Deja el mundo más limpio de como lo encontraste. Siempre que veas un código que puede ser mejorado/limpiado, dedícale un tiempo para hacerlo. De este modo, toda deuda técnica que pueda existir se va a ir pagando gradualmente y el código irá mejorando vez a vez.

### Utiliza variables explicativas.
Una de las formas más poderosas de hacer un programa legible es partir sus cálculos en valores intermedios que son guardados en variables con nombres significativos. Considerar el siguiente ejemplo:

```
Matcher match = headerPattern.matcher(line);
if (match.find()) {
	String key = match.group(1);
	String value = match.group(2);
	headers.put(key.toLowerCase(), value);
}
```

El simple uso de variables auto explicativas hace evidente que el primer grupo encontrado es la llave y el segundo grupo es el valor. Generalmente el contar con más variables auto explicativas es mejor que tener menos variables. Es extraordinario como un módulo opaco puede súbitamente convertirse en claro simplemente a partir los cálculos en valores intermedios bien nombrados.

### Múltiples lenguajes en un archivo fuente.
Los ambientes de programación actuales hacen posible poder poner muchos diferentes tipos de lenguajes en un solo archivo. Por ejemplo, un archivo fuente de Java puede contener fragmentos de XML, HTML, JavaDoc, Inglés, Javascript y demás. En otro ejemplo, adicionalmente a HTML un JSP puede contener código Java, etiquetas de librerías (spring mvc tags), JavaScript y demás. Esto es confuso en el mejor de los casos y descuidadamente lleno de parches en el peor de los casos. Lo ideal para un archivo fuente es tener un, y sólo un, lenguaje. En realidad, probablemente tendremos que usar más de uno. Pero debemos de tomar medidas para minimizar la cantidad de lenguajes en nuestros archivos fuentes.

### Ignorar advertencias/mecanismos de seguridad.
Ignorar cada una de las advertencias o mecanismos de seguridad puede traer consigo resultados catastróficos, es muy riesgoso. Desactivar algunas (¡o todas!) advertencias (warnings) de compilación pudieran ayudar a construir el proyecto exitosamente, pero con el riesgo de sesiones infinitas de depuración (debug). Ignorar (@Ignore) pruebas unitarias y decirte a ti mismo que las harás pasar correctamente después es tan malo como pensar que tus tarjetas de crédito son dinero gratis.

## Ambiente.
### Compilación y construcción del proyecto sólo requiere un paso.
Construir un proyecto debe ser una simple operación trivial. No se debe necesitar una secuencia de comandos o scripts dependientes de un ambiente para compilar. Tampoco se debe de buscar por todos aquellos JARs o archivos XML que el sistema requiera. Debes de ser capaz de hacer check out con un simple comando y después compilar con otro simple comando para construirlo.

```
svn co miSistema
cd miSistema
mvn clean install
```

###  Ejecución de pruebas en un solo paso.
Debes de ser capaz de correr todas las pruebas unitarias con un simple comando. Adicionalmente se debe de poder correr todos las pruebas unitarias al dar un sólo click en un botón dentro del IDE. Ser capaz de correr todas las pruebas es tan fundamental e importante que su ejecución debe ser rápida, fácil y obvia de hacer.

```
mvn test
```

### Sistema de control de versiones.
Siempre utilizar un sistema de control de versiones (subversion, git, RTC). Los sistemas de control de versiones son muy buenos para recordar quien agregó que y cuando.

### Integración continua.
Asegurar integridad con un servidor de Integración Continua responsable de compilar y ejecutar las pruebas unitarias durante cada cierto intervalo de tiempo o cada commit realizado sobre el sistema de control de versiones.

## Inicialización.
### Desacoplar la construcción de la ejecución.
Primero, considera que la construcción es un proceso muy diferente al uso. Los sistemas de software deben separar el proceso de arranque, cuando los objetos de la aplicación son construidos y las dependencias son alambradas, de la lógica de ejecución que toma el control después del arranque.

Desafortunadamente, la mayoría de las aplicaciones no separan estos asuntos. El código del proceso de arranque está mezclado con la lógica de ejecución. He aquí un típico ejemplo:

```
public Service getService() {
	if (service == null) {
		service = new MyServiceImpl(...);
	}
	return service;
}
```

El problema con este código es que se tiene en hard-code la dependencia con MyServiceImpl y todo lo que el constructor requiere. No podemos compilar sin antes resolver estas dependencias, inclusive si no ocupamos el objeto una sola vez en tiempo de ejecución.

La ejecución de las pruebas puede verse afectada. Si deseamos probar con algún objeto mock no será posible porque tenemos mezclada la construcción del objeto con el código funcional.

Debemos modularizar el proceso de arranque de la lógica normal en tiempo de ejecución. Existen algunas técnicas que nos ayudan a separar la construcción del uso:

#### Separation of main.
Consiste simplemente en mover todos los aspectos de construcción al `main` y diseñar el resto del sistema asumiendo que todos los objetos han sido construidos y cableados apropiadamente.

#### Fábricas.
En ocasiones, necesitamos que la aplicación sea responsable de cuándo crear un objeto. Para estos casos podemos usar algún patrón de construcción como AbstractFactory [2] para dar a la aplicación del control sobre cuándo crear el objeto, pero mantener los detalles de la construcción separada del código de la aplicación.

#### Inyección de dependencias.
Un mecanismo poderoso para separar la construcción del uso es la inyección de dependencias, DI (Dependency Injection). En este contexto, un objeto no tiene responsabilidad alguna de instanciar sus dependencias por sí mismo. En lugar de eso, le pasa la responsabilidad a otro mecanismo responsable de la construcción y el amarre de cada una de las dependencias, siempre y cuando estas implementen las interfaces apropiadas.

Desacoplar completamente la fase de construcción/inicialización de la de ejecución ayuda a simplificar el comportamiento en tiempo de ejecución.

## Diseño.
### Se preciso.
Esperar que la primer coincidencia sea la única en una consulta (query) es probablemente muy inocente. Utilizar números de punto flotante para representar dinero o moneda es casi un crimen. Evitar candados (locks) y/o manejo de transacciones porque piensas que una actualización concurrente no es probable es dispararse a los pies en el mejor de los casos.

Cuando se tome una decisión en tu código, asegúrate de que se haga con precisión. Conoce porqué lo has hecho y cómo tratarás las posibles excepciones. No permitas que la flojera elimine precisión sobre tus decisiones. Si decides llamar a una función que pudiera regresar null, asegúrate que revisas por null en el resultado. Si tu consulta de lo que piensas solamente contiene un registro en la base de datos, asegúrate que tu código se asegura que no existan otros. Si necesitas trabajar con dinero utiliza enteros, o una clase Money que utilice enteros, y maneja el redondeo apropiadamente. Si existe la posibilidad de una actualización concurrente, asegúrate de que implementas algún tipo de mecanismo de bloqueo.

Ambigüedades e imprecisiones en el código pueden ser el resultado de desacuerdos o flojera. En cualquiera de los casos estos deben ser eliminados.

### Preferir polimorfismo sobre `if/else` o `switch/case`.
Las sentencias o cláusulas switch son probablemente apropiadas en las partes del sistema donde agregar nuevas funciones es más probable que agregar nuevos tipos. Dado esto, podemos definir dos puntos para preferir el polimorfismo sobre el uso de sentencias switch.

Primero, la mayoría de la gente usa sentencias switch porque es la obvia solución de fuerza bruta, no porque es la solución correcta para la situación. Por lo que esta heurística está aquí para recordarnos el considerar polimorfismo antes de usar un switch.

Segundo, los casos donde las funciones son más volátiles que los tipos son relativamente raros. Por lo que cada switch es sospechoso. En general hay que usar la regla UN SOLO SWITCH: No debe de existir más de un switch para un cierto tipo de selección. Los cases en ese único switch deben crear objetos polimórficos que tomen el lugar de cláusulas switch en el resto del sistema.

###  Campos que no definen estado.
Son campos que mantienen información que no pertenece al _estado_ de la instancia pero que son usados, por ejemplo, para mantener información temporalmente durante la invocación de un método privado o de utilería que vive en la misma clase. En tal caso, utilizar variables locales, parámetros o extraerlos a una clase que abstraiga la acción deseada.

###  Mantener datos de configuración en altos niveles.
Si tienes una constante como un valor o configuración por defecto y se sabe que se espera a un alto nivel de abstracción, no lo ocultes en una función de bajo nivel. Expónlo como un argumento de la función de bajo nivel llamado desde la función de alto nivel. Considera el siguiente código:

```
public static void main(String[] args) throws Exception {
	Arguments arguments = parseCommandLine(args);
	...
}

public class Arguments {
	public static final String DEFAULT_PATH = ".";
	public static final String DEFAULT_ROOT = " FitNesseRoot";
	public static final int DEFAULT_PORT = 80;
	public static final int DEFAULT_VERSION_DAYS = 14;
	...
}
```

Los argumentos de línea de comandos son leídos en la primer línea. Los valores por defecto de esos argumentos están definidos en la parte superior de la clase Argument. Como resultado, no tienes que ir buscando en niveles bajos del sistema por sentencias como esta:

```
if (arguments.port == 0) // use 80 by default
```

Las constantes de configuración permanecen en la parte más alta del sistema y son fáciles de cambiar. Además estas son pasadas al resto de la aplicación y los niveles bajos de la aplicación no son dueños de los valores de estas constantes.

## Concurrencia.
### Separar el código concurrente del funcional - SRP (Single Responsibility Principle)
El SRP indica que un método, clase o componente debe tener una, y sólo una, razón para cambiar. El diseño concurrente es suficientemente complejo y por lo tanto merece ser separado del resto de código. Desafortunadamente, es tan común para la implementación de concurrencia que sea mezclado directamente con otro código productivo. Aquí unas consideraciones:

* Código relacionado a concurrencia tiene su propio ciclo de vida de desarrollo, cambio y puesta a punto.
* Código relacionado a concurrencia tiene sus propios retos, los cuales son diferentes y frecuentemente más difíciles que código no relacionado a concurrencia.
* La cantidad de formas en que código concurrente mal escrito pueda fallar, genera un reto suficientemente complicado como para agregar o sobrecargarlo con código funcional de la aplicación.

**Recomendación** Mantén tu código relacionado a concurrencia separado del resto de código.

## Dependencias.
### Acoplamiento artificial.
En general un acoplamiento artificial es un acoplamiento entre dos módulos que no tienen un propósito directo. Es el resultado de colocar una variable, constante o función en una ubicación temporalmente conveniente, aunque inapropiada. Esto se hace sin cuidado y con flojera.

Las cosas que no dependen una de la otra no deben ser acopladas artificialmente. Por ejemplo, enums generales no deben ser contenidos dentro de clases más específicas ya que esto fuerza a que la aplicación sepa acerca de estas clases específicas. Lo mismo ocurre para funciones static de uso general que son declaradas en clases específicas.

Recomendación. Toma el tiempo necesario para decidir en donde deben ser declaradas las funciones, constantes y variables. No sólo las avientes en el lugar más conveniente y a la mano y después las dejes ahí.

### Encapsulamiento.
Los métodos de una clase deben de estar interesados en las variables y métodos de la clase a la que pertenecen y no en las variables y métodos de otras clases. Cuando un método usa métodos de acceso o de modificación de cualquier otro objeto para manipular la información dentro de ese objeto, entonces se está violando el encapsulamiento de la clase del otro objeto. Se desea que se estuviese dentro de la otra clase para que se pueda acceder a las variables y manipularlo. Se debe de mantener el encapsulamiento. Por ejemplo:

```
public class HourlyPayCalculator {

	public Money calculateWeeklyPay(HourlyEmployee e) {
		int tenthRate = e.getTenthRate().getPennies();
		int tenthsWorked = e.getTenthsWorked();
		int straightTime = Math.min(400, tenthsWorked);
		int overTime = Math.max(0, tenthsWorked − straightTime);
		int straightPay = straightTime * tenthRate;
		int overtimePay = (int)Math.round(overTime * tenthRate * 1.5);

		return new Money(straightPay + overtimePay);
	}

}
```

El método `calculateWeeklyPay` accede el objeto `HourlyEmployee` en la primera línea para obtener los datos que requiere para realizar el cálculo. Este método viola el alcance de `HourlyEmployee` ya que “desea” estar dentro de la clase `HourlyEmployee`.

En igualdad de condiciones, queremos eliminar la violación del encapsulamiento porque expone los internos de una clase hacia otra. Aunque, algunas veces es necesario. Considerando lo siguiente:

```
public class HourlyEmployeeReport {
	
	private HourlyEmployee employee ;
	
	public HourlyEmployeeReport(HourlyEmployee e) {
		this.employee = e;
	}

	String reportHours() {
		return String.format("Name: %s \tHours: %d.%1d\n",
			employee.getName(), employee.getTenthsWorked() / 10,
			employee.getTenthsWorked() % 10);
	}

}
```

Claramente, el método reportHours viola el encapsulamiento de la clase HourlyEmployee. Por otro lado, no queremos que HourlyEmployee sepa acerca del formato del reporte. Mover el formato de la cadena hacia dentro de la clase HourlyEmployee violaría varios principios del diseño orientado a objetos. Se acoplaría HourlyEmployee al formato del reporte, siendo frágil ante cambios en ese formato.

## Nomenclatura.
### Prefijos y codificaciones.
Los nombres no deben ser codificados con información de tipo o de alcance. Prefijos como `m_` o `f` son inútiles en los ambientes actuales. Además codificaciones de proyecto y/o subsistema como `vis_` (de vista) distraen y son redundantes. Nuevamente, los ambientes actuales proveen toda la información sin necesidad de mutilar los nombres: Mantén los nombres libres de la contaminación (notación Húngara).

La notación que se debe adoptar es la notación camello sin incluir prefijos ni codificaciones en ella.

### Nombres descriptivos.
No seas muy rápido al escoger un nombre. Asegúrate de que el nombre sea descriptivo. Recuerda que los significados tienden a irse a la deriva mientras el software evoluciona, así que frecuentemente reevalúa que los nombres que escogiste sean los apropiados. Esto no es una recomendación para sentirse bien. Los nombres en el software representan el 90 por ciento de lo que hace un software leíble. Necesitas tomarte el tiempo para escoger el nombre sabiamente y mantenerlos relevantes. Los nombres son tan importantes como para tratarlos sin cuidado. Considera el siguiente código. ¿Qué hace? Si se muestra el código con nombres bien elegidos, hará perfecto sentido para ti, pero esto es sólo un conjunto de símbolos y números mágicos:

```
public int x() {
	int q = 0;
	int z = 0;
	for (int kk = 0; kk < 10; kk++) {
		if (l[z] == 10) {
			q += 10 + (l[z + 1] + l[z + 2]);
			z += 1;
		} else if (l[z] + l[z + 1] == 10) {
			q += 10 + l [z + 2];
			z += 2;
		} else {
			q += l[z] + l[z + 1];
			z += 2;
		}
	}
	return q;
}
```

A continuación se presenta el código como debe ser escrito. Este fragmento es de hecho menos completo que el código previamente mostrado. Aún así puedes inferir inmediatamente lo que está tratando de hacer y tú muy probablemente podrías terminar de escribir las funciones faltantes basado en lo que inferiste. Los números mágicos ya no son más mágicos y la estructura del algoritmo es convincentemente descriptiva.

```
public int score() {
	int score = 0;
	int frame = 0;
	for (int frameNumber = 0; frameNumber < 10; frameNumber++) {
		if (isStrike(frame)) {
			score += 10 + nextTwoBallsForStrike(frame);
			frame += 1;
		} else if (isSpare(frame)) {
			score += 10 + nextBallForSpare(frame);
			frame += 2;
		} else {
			score += twoBallsInFrame(frame);
			frame += 2;
		}
	}
	return score;
}
```

### Escoge un nivel apropiado al nivel de abstracción.
No escojas nombres que comunican la implementación; escoge nombres que reflejen el nivel de abstracción de la clase o método en el que estás trabajando. Esto es difícil de hacer, principalmente porque la gente es muy buena para mezclar los niveles de abstracción. Cada vez que revises tu código, es probable que encuentres alguna variable que esté nombrada a muy bajo nivel. Por ejemplo:

```
public interface Modem {
	boolean dial(String phoneNumber);
	boolean disconnect();
	boolean send(char c);
	char recv();
	String getConnectedPhoneNumber();
}
```

De primer vistazo se ve bien. Todos los métodos aparentan ser apropiados y en general para muchos casos lo son. Pero ahora considera una aplicación que utiliza módems que no son conectados mediante marcación, como algún módem por cable. Quizás utilicen algún número de puerto o algún switch. Claramente la noción de números telefónicos está al nivel de abstracción incorrecto. Una mejor estrategia de nombrado para este escenario sería:

```
public interfaceModem {
	boolean connect(String connectionLocator);
	boolean disconnect();
	boolean send(char c);
	char recv();
	String getConnectedLocator();
}
```

Ahora los nombres no hacen referencia a números telefónicos ni a marcación. Aún así, estos pueden ser usados para números telefónicos u otro protocolo o estrategia de conexión.

### Nombrar interfaces basado en lo que hacen.
El nombre de una interfaz debe ser derivado de la funcionalidad y el uso dado por el cliente, como `UserDao`

### Nombrar clases basado en cómo implementan sus interfaces.
El nombre de una clase debe reflejar cómo es que cumple con la funcionalidad proveída por su interfaz, como `JdbcUserDao implements UserDao` o `FileSystemUserDao implements UserDao`

### Nombrar métodos basado en lo que hacen.
El nombre de un método debe describir qué se hace, no cómo se hace. Por ejemplo:

```
Date newDate = date.add(5);
```

¿Esperarías que se agreguen cinco días a la fecha? ¿O son semanas u horas? ¿La instancia date es cambiada o la función simplemente regresa una nueva instancia de Date sin cambiar la actual?

_No es posible saber qué hace la función_

Si la función agrega cinco días a la fecha y la cambia, entonces debe ser llamada `addDaysTo` o `increaseByDays`. Si, por el otro lado, la función regresa una nueva fecha que es cinco días posteriores pero no cambia la instancia `date`, debe ser nombrado `daysLater` o `daysSince`. Si tienes que mirar a la implementación (o documentación) de la función para saber qué hace, entonces debes de trabajar en encontrar un mejor nombre o reacomodar la funcionalidad para que pueda ser colocada en funciones con mejores nombres.

### Nombres no ambiguos.
Escoge nombres que definen la funcionalidad de métodos o variables sin ambigüedades. Considera el siguiente ejemplo:

```
private String doRename() throws Exception {
	if (refactorReferences) {
		renameReferences();
	}
	renamePage();
	pathToRename.removeNameFromEnd();
	pathToRename.addNameToEnd(newName);
	return PathParser.render(pathToRename);
}
```

El nombre del método `doRename` no dice lo que hace mas que en términos muy vagos. ¡Además existe una función llamada `renamePage` dentro de la función llamada doRename! ¿Qué diferencia encuentras entre las 2 funciones? Ninguna prácticamente.

`renamePageAndOptionallyAllReferences` sería un mejor nombre para la función. Pudiera aparentar ser un nombre largo, y lo es, pero el valor otorgado por su autoexplicación es más valioso que la cantidad de caracteres utilizados en su nombre.

### Usar nombres largos para un largo alcance.
La longitud de un nombre debe tener una relación directa con su alcance. Puedes usar nombres de variables muy cortos para alcances pequeños, pero nombres largos para grandes alcances. Nombres de variables como i y j están bien si su alcance es, digamos 5 líneas. Considera este segmento:

```
private void rollMany(int n, int pins) {
	for (int i =0; i < n; i++) {
		g.roll(pins);
	}
}
```

Esto es perfectamente claro y sería muy confuso si la variable i fuera reemplazado con algo molesto como rollCount. Por otro lado, las variables y funciones con nombres cortos pierden su significado en grandes distancias. Así que mientras más grande sea el alcance de un nombre, más grande y preciso el nombre debe ser.

**En general** se pueden seguir estas reglas:

* Nombres largos para campos y parámetros.
* Nombres cortos para variables locales o de ciclos/bucles.

### Nombres describiendo efectos secundarios.
Los nombres deben describir todo lo que una función, variable o clase hace. No ocultes efectos secundarios con un nombre. No utilices verbos en su forma simple para describir una función que hace más que una simple acción. Por ejemplo:

```
public ObjectOutputStreamgetOos() throws IOException {
	if (m_oos == null) {
		m_oos = new ObjectOutputStream(m_socket.getOutputStream());
	}
	return m_oos;
}
```

La función hace un poco más que la obtención de un “oos”; esta crea el “oos” si es que no ha sido creado anteriormente. Así, un mejor nombre debería ser `createOrReturnOos`.

## Legibilidad.
### Encapsular condiciones de frontera.
Las condiciones de frontera son difíciles de seguir. Coloca su procesamiento en un solo lugar.

No dejes que se fuguen o estén plagadas por todo el código. No queremos hordas de +1s y -1s dispersos

en el código. Considera el siguiente ejemplo:

```
if (level + 1 < tags.length) {
	parts = new Parse(body, tag s, level + 1, offset + endTag);
	body = null;
}
```

Nota que `level + 1` aparece dos veces. Esto es una condición de frontera que debe ser encapsulada en una variable llamada algo así como `nextLevel`:

```
int nextLevel = level + 1;
if (nextLevel < tags.length) {
	parts = new Parse(body, tags, nextLevel, offset + endTag);
	body = null;
}
```

### Preferir value objects a tipos de dato primitivo.
En lugar de pasar tipos primitivos como String o integers, usar value objects dedicados. Por ejemplo, Price en lugar de double o AbsolutePath en lugar de String. Con esto somos más específicos en lo que necesitamos y existe menos probabilidad de recibir un valor incorrecto. Adicionalmente se tiene encapsulada la representación del valor y al cambiarla, por ejemplo utilizar long en lugar de double para representar el precio, el cambio se mantiene en un solo lugar.

### Intenciones oscurecidas.
Se quiere código que sea tan expresivo como sea posible. Notación húngara (hungarian notation), números mágicos, entre otros opacan u oscurecen la intención del autor. Por ejemplo:

```
public int m_otCalc() {
	return iThsWkd * iThsRte
		+ (int) Math.round(0.5 * iThsRte * Math.max(0, iThsWkd − 400));
}
```

Esto aunque es muy pequeño, es muy denso. Vale la pena dedicar tiempo para hacer visible hacia los lectores de la intención de la función o del algoritmo.

### Constantes vs Enumeraciones.
Ahora que desde Java 5 existen las enums, ¡úsalas! No sigas usando el viejo truco de public static final ints. El significado de los ints se puede perder. El significado de los enums no, porque pertenece a una enumeración que está nombrada. Mejor aún, estudia cuidadosamente la sintaxis de los enums. Pueden tener métodos y campos. Esto los hace herramientas muy poderosas que permiten mucha mayor expresión y flexibilidad que los ints. Considera el siguiente código:

```
public class HourlyEmployee extends Employee {
	private int tenthsWorked ;

	HourlyPayGrade grade ;
	public Money calculatePay() {
		int straightTime = Math.min(tenthsWorked, TENTHS_PER_WEEK);
		int overTime = tenthsWorked − straightTime;

		return new Money(grade.rate() * (tenthsWorked + OVERTIME_RATE * overTime));
	}
	...
}

public enum HourlyPayGrade {

	APPRENTICE {
		public double rate() {
		return 1.0;
		}
	},

	LEUTENANT_JOURNEYMAN {
		public double rate() {
			return 1.2;
		}
	},

	JOURNEYMAN {
		public double rate() {
			return 1.5;
		}
	},

	MASTER {
		public double rate() {
			return 2.0;
		}
	};

	public abstract double rate();

}
```

Cada empleado cuenta con un HourlyPayGrade que es un enum donde se tiene definida la tasa para llevar a cabo el cálculo de su salario dependiendo del grado con el que cuente el empleado.

## Comentarios
### Propósito de un comentario.
Su propósito es explicar el código que no se explica por sí mismo. Además pueden/deben advertir sobre consecuencias o decisiones tomadas.

### Información inapropiada.
Es inapropiado para un comentario almacenar información que es mejor almacenada en un diferente tipo de sistema como el sistema de control de versiones, el sistema de seguimiento de incidencias (JIRA) o algún otro sistema. Historiales de revisión, por ejemplo, simplemente agregan volúmenes de texto nada interesantes.

**En general**, datos como autores, fecha de última modificación, número de incidente y demás no deben de aparecer en los comentarios. Los comentarios deben ser exclusivamente reservados para notas técnicas acerca del código y diseño.

### Comentario obsoleto.
Un comentario que se ha hecho viejo, irrelevante o incorrecto es obsoleto. Los comentarios se vuelven obsoletos rápidamente. Es mejor no escribir un comentario que se convertirá en obsoleto. Si encuentras un comentario obsoleto, es mejor actualizarlo o borrarlo tan pronto como sea posible. Los comentarios obsoletos tienden a confundir y ser irrelevantes al código.

**Recuerda**, sólo coloca comentarios al código que no se puede explicar por sí mismo.

### Comentario redundante.
Un comentario es redundante si describe algo que adecuadamente se describe así mismo. Por ejemplo:

```
// Si tiene 18 años o más.
if (edad >= 18) {
	...
}
```

Otro ejemplo es un Javadoc que no dice nada adicional (en ocasiones menos que) a la firma del método:

```
/**
 *
 * @param sellRequest
 * @return
 * @throws ManagedComponentException
 */
public SellResponse beginSellItem(SellRequest sellRequest) throws ManagedComponentException
```

Los comentarios deben decir cosas que el código no puede decir por sí mismo.

### Comentarios escritos pobremente.
Si vas a escribir un comentario, tómate el tiempo para asegurarte que es el mejor comentario que puedes escribir. Selecciona tus palabras cuidadosamente. Usa correctamente la gramática y puntuación.

### Comentario de un commit.
Alguna vez has revisado el historial de cambios del sistema de control de versiones y pensado, ¿Qué demonios pasó aquí? Por ejemplo:

```
fix test.
```

o

```
added logged out user page.
```

o

```
changes.
```

¿Qué se supone que debo de hacer con eso? Esto no me dice nada de lo que está sucediendo en el proyecto. Si veo los comentarios seis meses a partir de ahora no tendría ni idea a qué prueba se refiere  `fix test` o a que página se refiere _user page_. De hecho, justo ahora no tengo la capacidad para descubrir el significado.

El mensaje de commit describe los cambios que se han agregado al proyecto y ayudan al usuario a entender la historia de los archivos contenidos en el repositorio. Por ende, este mensaje debe ser descriptivo e informativo sin repetir los cambios en el código. Existen tres cosas básicas que se deben de incluir:

1. Título o resumen.
2. Descripción detallada.
3. Número de seguimiento, ticket o id de incidente si aplica.

Por ejemplo:

> Corrige error donde el usuario no puede registrarse en el sistema.

> Los usuarios no podían registrarse al sistema si no habían visitado antes la página de precios porque se esperaba que existiera la información de seguimiento, misma que se envía al log después de que el usuario se registra. Se corrige agregando una validación para asegurar que si la información de seguimiento no existe que no se intente escribir en el log.

> JIRA PRY-136

Cada uno de los elementos mencionados deben ir separados por una línea en blanco. El cuerpo debe principalmente describir el porqué del cambio, como se soluciona y los efectos secundarios si es que existen.

Adicionalmente el mensaje debe estar en sujeto tácito, por ejemplo, “Corrige error donde ...” en lugar de “Corregido error donde ...”. Pues intentamos describir lo que hace el commit, no lo que se hizo.

### Código muerto.
Código muerto es aquel que no se ejecuta. Puede estar comentado y también se puede encontrar en el cuerpo de un if que evalúa una condición que nunca pasa. Lo encuentras en métodos de utilería que nunca son invocados o en condiciones de un switch/case que nunca ocurren. Nadie sabe que tan viejo es, ni tampoco saben si es valioso para el sistema. Peor aún, nadie lo borrará porque todos asumen que alguien más lo necesita o tiene planes para él.

El código comentado se queda ahí y le salen raíces, siendo cada vez más y más irrelevante con cada día que pasa. El problema con código muerto es que después de un rato comienza a apestar. Mientras más viejo es, más fuerte y amargo su olor es. Esto es porque código muerto no se actualiza completamente cuando se cambia el diseño.

Continua compilando, pero no sigue las nuevas convenciones o reglas. Fue escrito en algún momento cuando el sistema era diferente. Cuando veas código muerto, haz la mejor cosa. Dale un entierro decente y bórralo del sistema. No te preocupes, el sistema de control de versiones lo mantendrá. Si realmente alguien lo necesita, podrá obtenerlo de una versión anterior.

## Métodos.
### Métodos con muchos argumentos.
Difícilmente existe algo más abominable que un argumento false al final de una invocación a una función. ¿Qué significa? ¿Qué cambiaría si fuese true? No sólo es difícil recordar cuál es el propósito del argumento sino que indica claramente que el método está haciendo más de una cosa. Los argumentos booleanos son una simple representación de la flojera de dividir una función muy grande en varias funciones pequeñas. Considerar:

```
public int calculateWeeklyPay(boolean overtime) {
	int tenthRate = getTenthRate();
	int tenthsWorked = getTenthsWorked();
	int straightTime = Math.min(400, tenthsWorked);
	int overTime = Math.max(0, tenthsWorked − straightTime);
	int straightPay = straightTime * tenthRate;
	double overtimeRate = overtime ? 1.5 : 1.0 * tenthRate;
	int overtimePay = (int) Math.round(overTime * overtimeRate);
	return straightPay + overtimePay;
}
```

Se invoca esta función con `true` si se paga tiempo extra a una tasa de una vez y medio y con `false` si se pagará a tasa normal. Es suficientemente malo que debas recordar que significa `calculateWeeklyPay(false);` cada que te topes con este segmento de código y tengas que revisar la documentación o la implementación.

Se desperdició una oportunidad para escribir lo siguiente:

```
public int straightPay() {
	return getTenthsWorked() * getTenthRate();
}

public int overTimePay() {
	int overTimeTenths = Math.max(0, getTenthsWorked() − 400);
	int overTimePay = overTimeBonus(overTimeTenths);
	return straightPay() + overTimePay;
}

private int overTimeBonus(int overTimeTenths) {
	double bonus = 0.5 * getTenthRate() * overTimeTenths;
	return (int) Math.round(bonus);
}
```

Los argumentos no necesariamente son `boolean`. Pueden ser `enums`, `integers` o cualquier otro tipo de argumento que es usado para seleccionar el comportamiento dentro de la función.

**En general** es mejor tener muchas funciones que pasar algún código o clave a una función para seleccionar el comportamiento. De cualquier manera quien invoca sabe qué operación debe ejecutar.

### Static inapropiado.
Math.max(double a, double b) es un buen método estático. No opera sobre alguna instancia; de hecho, sería tonto tener que hacer new Math().max(a,b) o inclusive a.max(b). Toda la información que max requiere viene de los dos parámetros. Además, existe muy poca probabilidad de que queramos que Math.max sea polimórfica.

En ocasiones escribimos funciones estáticas que no deben serlo. Por ejemplo:

```
HourlyPayCalculator.calculatePay(employee, overtimeRate)
```

Aparentemente es una buena función estática. No opera con ningún objeto en particular y obtiene toda la información de sus argumentos. Aunque, existe una razonable probabilidad de que queramos que esta función sea polimórfica. Quizás queramos implementar diferentes algoritmos para calcular el pago, por ejemplo, OvertimeHourlyPayCalculator. En este caso la función no debería ser static. Debería ser una función miembro de Employee.

**En general**, se debería de preferir métodos no estáticos sobre los estáticos. En caso de duda, haz la función no estática. Si realmente quieres que una función sea estática, asegúrate que no hay ninguna probabilidad de que se comporte de manera polimórfica.

### Métodos muertos.
Los métodos que nunca son invocados deben ser descartados. Manteniendo código muerto en el sistema es un desgaste. No tengas miedo de eliminar el método. Recuerda, tu sistema de control de versiones lo recordará por ti.

### Métodos deben hacer una sola cosa.
Siempre es tentador crear métodos que tengan múltiples secciones que ejecutan un conjunto de operaciones. Los métodos de este tipo hacen más de una cosa y deben ser convertidos en muchas funciones pequeñas, donde cada una de ellas hace una cosa. Por ejemplo:

```
public void pay() {
	for (Employee e : employees) {
		if (e.isPayday()) {
			Money pay = e.calculatePay();
			e.deliverPay(pay);
		}
	}
}
```

Este pequeño segmento de código hace tres cosas. Itera por todos los empleados, checa si el empleado debe recibir un pago y luego le paga. Este código sería mejor escrito de la siguiente manera:

```
public void pay ( ) {
	for (Employee e : employees) {
		payIfNecessary(e);
	}
}

private void payIfNecessary(Employee e) {
	if (e.isPayday()) {
		calculateAndDeliverPay(e);
	}
}

private void calculateAndDeliverPay(Employee e) {
	Money pay = e.calculatePay();
	e.deliverPay(pay);
}
```

Cada una de estas funciones hace una sola cosa.

**Recomendación**. Si bien el ejemplo puede caer en una exageración al dividir una tarea simple, que es bastante entendible, en 3 métodos, debemos ser capaces de identificar en qué momento dividir un método. Nos podemos apoyar en la complejidad ciclomática, donde si la excedemos es altamente probable que se requiera dividir el método. Otra regla de oro es saber si contamos con duplicados.

### Complejidad innecesaria.
En ocasiones es muy fácil agregar complejidad innecesariamente al código. Eso puede ser motivado por tratar de resolver un algoritmo o funcionalidad en un sólo método trayendo como resultado la anidación de varios if usados en conjunto con bucles while.

Desafortunadamente la complejidad agregada hace más difícil de comprender el código. Por lo tanto, extender y cambiar el código resulta en un mayor esfuerzo que el necesario.

En general para estos casos se debe de mantener métodos pequeños que invoquen a submétodos con una tarea bien definida y que encapsulan los distintos caminos de ejecución.

## Condicionales.
### Encapsular condiciones.
Lógica booleana es suficientemente difícil de entender sin tener que verla en el contexto de un `if` o `while`. Extrae funciones que explican la intención de la condicional. Por ejemplo:

```
if (this.shouldBeDeleted(timer))
```

ès preferible a:

```
if (timer.hasExpired() && !timer.isRecurrent())
```

### Condicionales positivas.
Condicionales negativas son simplemente más difíciles de entender que las positivas. Por lo tanto, en la medida de lo posible, las condicionales deben ser expresadas como positivas. Por ejemplo:

```
if (buffer.shouldCompact())
```

es preferible a:

```
if (!buffer.shouldNotCompact())
```

## Enemigos del mantenimiento.
### Duplicados.
Esta es una de las reglas más importantes definidas en estos lineamientos y deberás tomarla con mucha seriedad. Existe un principio utilizado muy comúnmente por autores quienes escriben sobre diseño de software llamado DRY (Don’t repeat yourself). ¡Usa este principio!.

Cada vez que veas código duplicado, representa una oportunidad desperdiciada para la abstracción. Este duplicado puede convertirse en una subrutina o quizás en alguna otra clase. Al eliminar este duplicado con una abstracción, se incrementa el vocabulario del lenguaje de tu diseño. Otros programadores pueden usar las herramientas que acabas de crear, la codificación es más rápida y menos propensa a errores simplemente porque has elevado el nivel de abstracción.

La forma más obvia de duplicado es cuando ves pedazos de código iguales que aparentan como si algunos programadores agarraron el mouse, seleccionaron, copiaron y pegaron el código una y otra vez. Esto puede ser reemplazado por simples métodos.

Otra forma más sutil es los switch/case o if/else que aparecen una y otra vez en varios módulos, siempre evaluando las mismas condiciones. Estos deben ser reemplazados con polimorfismo.

Aún más sutil son los módulos que tienen algoritmos similares, pero que varían en algunas líneas de código. Aún así es un duplicado y deberían ser resueltos utilizando los patrones método plantilla (Template Method) o estrategia (Strategy).

Recuerda que la orientación a objetos por sí misma es una estrategia para organizar los módulos y eliminar los duplicados. Sin sorpresas, la programación estructurada tiene el mismo objetivo. Así que encuentra y elimina los duplicados cada vez que puedas.

### Reemplazar números mágicos con constantes.
Probablemente esta es una de las reglas más viejas en el desarrollo de software. En general es una mala idea tener números en crudo en el código. Se deben ocultar detrás de constantes bien nombradas. Por ejemplo, el número 86,400 debe ser ocultado detrás de la constante SECONDS_PER_DAY . Si vas a imprimir 55 líneas por página, entonces la constante 55 debe ser ocultada por la constante LINES_PER_PAGE. Algunas constantes son tan fácil de reconocer que no siempre necesitan un nombre siempre y cuando sean usadas en conjunto con código auto explicativo. Por ejemplo:

```
int dailyPay = hourlyRate * 8;
double circumference = radius * Math.PI * 2;
```

¿Realmente necesitamos las constantes `WORK_HOURS_PER_DAY` y `TWO` en los ejemplos mencionados? Claramente el último caso es absurdo. Existen algunas fórmulas donde las constantes simplemente son mejor escritas como números en crudo. 

Se puede cuestionar `WORK_HOURS_PER_DAY` ya que dependiendo de leyes o convenciones pudiera cambiar. Constantes como `3.141592653589783` son bastante bien conocidas y fácilmente reconocibles. Sin embargo, la probabilidad de error es tan grande como para dejarlos en crudo. Cada vez que alguien ve `3.1415926535890783`, saben que es π sin darse cuenta que hay un error en un dígito, ¿Lo pudiste notar?. Para estos casos, es bueno que `Math.PI` haya definido esto por nosotros. El término número mágico no solo se refiere a números. Aplica para cualquier valor que no sea auto describible.

## Manejo de excepciones.
### Usar excepciones en lugar de códigos de error.
En el pasado existían muchos lenguajes que no contaban con excepciones. En aquellos lenguajes las técnicas para el manejo y reporte de errores eran limitadas. Tú podías colocar una bandera de error o regresar un código de error que el invocador pudiera revisar. El siguiente código ilustra estos enfoques.

```
public classDeviceController {

...

public void sendShutDown() {
	DeviceHandle handle = getHandle(DEV1);
	// Check the state of the device
	if (handle != DeviceHandle.INVALID) {
		// Save the device status to the record field
		retrieveDeviceRecord(handle);
		// If not suspended, shutdown
		if (record.getStatus() != DEVICE_SUSPENDED) {
			pauseDevice(handle);
			clearDeviceWorkQueue(handle);
			closeDevice(handle);
		} else {
			logger.log("Device suspended. Unable to shutdown");
		}
	} else {
		logger.log("Invalid handle for: " + DEV1.toString());
	}
}
```

El problema con esos enfoques es que el invocador debe revisar por errores inmediatamente después de la invocación. Desafortunadamente es fácil de olvidar. Por esta razón es mejor arrojar una excepción cuando encuentras un error. El código que invoca es más limpio y su lógica no se ve opacada por el manejo de errores.

```
public void sendShutDown() {
	try {
		tryToShutDown();
	} catch (DeviceShutDownError e) {
		logger.log(e);
	}
}

private void tryToShutDown ( ) throws DeviceShutDownError {
	DeviceHandle handle = getHandle(DEV1);
	DeviceRecord record = retrieveDeviceRecord(handle);
	pauseDevice(handle);
	clearDeviceWorkQueue(handle);
	closeDevice(handle);
}

private DeviceHandle getHandle(DeviceID id) {
	...
	throw new DeviceShutDownError("Invalid handle for: " + id.toString());
	...
}
```

Nótese que es mucho más claro. No es sólo cuestión de estética. El código es mucho mejor porque dos responsabilidades que estaban atadas, el algoritmo para apagar y el manejo de errores, ahora están separadas. Puedes mirar cada una de las responsabilidades y entenderlas independientemente.

#### Impacto en el rendimiento.
Bien es sabido que arrojar una excepción es mucho más caro que regresar un código de error, aunque no se había cuestionado qué tan caro es. Dado, esto nos dimos a la tarea de comparar el rendimiento de ambos enfoques, arrojar una excepción y regresar un código de error.

Los resultados confirman que regresar un código de error es mucho más barato que arrojar una excepción. Si bien la diferencia puede ser hasta 25 veces más costosa, los tiempos obtenidos son en el orden de nanosegundos. Aproximadamente arrojar y cachar una excepción toma 580 nanosegundos y devolver un código de error alrededor de los 25 nanosegundos. Adicionalmente se pudo observar que la ejecución de un bloque try catch que no genera excepción alguna, no tiene una diferencia apreciable contra la ejecución del mismo código sin estar envuelto en el bloque try.

Con la ejecución de esta prueba podemos concluir que las ventajas que trae consigo el uso de excepciones para el manejo de errores tiene un mayor peso con respecto al impacto en el rendimiento de la aplicación, mismo que no genera una afectación sustancial en la mayoría de las aplicaciones, inclusive en aquellas de alto rendimiento.

Así que utiliza excepciones en lugar de códigos de error.

### Runtime vs Checked Exceptions.
Cuando las checked-exceptions fueron agregadas en la primera versión de Java, aparentaban ser una grandiosa idea. La firma de cada método listaría todas las excepciones que pudiera pasar hacia el invocador. Es más, estas excepciones eran parte del tipo del método. Tu código literalmente no compilaría si la firma no corresponde con lo que tu código podía hacer. Dado esto, tenemos que decidir si el precio de las checked-exceptions vale la pena.

¿Cuál precio? El precio de las checked-exception es una violación al principio Open/Closed. Si tú arrojas una checked-exception en un método en tu código y el catch está tres niveles arriba, debes declarar la excepción en cada uno de los métodos entre el método que la genera y el que la cacha. Esto significa que un cambio a un nivel bajo del software puede forzar cambios de firma en muchos altos niveles. Los módulos cambiados deben ser reconstruidos y desplegados nuevamente, inclusive si no les importa el cambio. El resultado neto es una cascada de cambios disparada desde los niveles más bajos hasta los más altos. Dado que el propósito de las excepciones es permitir el manejo de errores a distancia, es una vergüenza que las checked-exceptions rompen el encapsulamiento de esta forma.

En ocasiones las checked-exceptions pueden ser útiles si estás escribiendo una librería crítica: las debes de cachar y dejar el sistema recuperado. Pero en general en el desarrollo de aplicaciones el costo de las dependencias es más caro que los beneficios.

**Recomendación.** Prefiere el uso de no checked-exceptions (runtime)) sobre las checked-exceptions a menos que estés completamente seguro que la capa superior es la responsable de manejar la excepción.

### No regresar `null`
Una de las principales cosas que invitan a los errores a aparecer es regresar null. He aquí un ejemplo de código:

```
public void registerItem(Item item) {
	if (item != null) {
		ItemRegistry registry = peristentStore.getItemRegistry();
		if (registry != null) {
			Item existing = registry.getItem(item.getID());
			if (existing.getBillingPeriod().hasRetailOwner()) {
				existing.register(item);
			}
		}
	}
}
```

Si trabajas con código como este, pudiera no ser malo para ti, ¡pero lo es! Cuando regresamos `null`, estamos creando más trabajo para nosotros y llenando de problemas a los que invocan nuestro método. Todo lo que se necesita es olvidar validar contra `null` para mandar poner a la aplicación fuera de control.

Pudiste notar que no se revisó que no fuese null la variable persistentStore. Se pudo haber tenido un `NullPointerException` en tiempo de ejecución.

Es fácil decir que el problema con el código es que falta checar contra `null`, pero de hecho, el problema es que tiene muchas validaciones. Si estás tentado a regresar `null` en un método, considera mejor arrojar una excepción o regresar un objeto para casos especiales. Si estás invocando un método de un API de algún tercero que regresa null, considera encapsular o colocar un Proxy al método para arrojar una excepción o regresar un objeto para este caso especial.

#### Null object pattern.
En muchas ocasiones, los objetos especiales son un buen y fácil remedio. Imagina que tienes código como este:

```
List<Employee> employees = getEmployees();
if (employees != null) {
	for (Employee e : employees) {
		totalPay += e.getPay();
	}
}
```

En este momento, getEmployees puede regresar null, pero ¿Tiene que hacerlo? Si cambiamos getEmployee para que regrese una lista vacía, podemos mantener el código limpio:

```
List<Employee> employees = getEmployees();
for (Employee e : employees) {
	totalPay += e.getPay();
}
```

Afortunadamente, Java tiene Collections.emptyList() y regresa una lista inmutable predefinida que podemos utilizar para este propósito:

```
public List<Employee> getEmployees() {
	if (thereAreNoEmployees) {
		return Collections.emptyList();
	}
	...
}
```

Si tu código se ajusta a estos casos especiales, se minimizará la probabilidad de NullPointerExceptions y tu código será más limpio.

### No pasar `null`.
Regresar `null` en los métodos es malo, pero pasar `null` a los métodos es peor. A menos que estés trabajando con una API que espere que le pases un `null`, debes de evitar pasar `null` en tu código siempre que sea posible.

Revisemos un ejemplo para ver el porqué. Aquí se muestra un simple método:

```
public class MetricsCalculator {
	public double xProjection(Point p1, Point p2) {
		return(p2.x − p1.x) * 1.5;
	}
	...
}
```

¿Qué pasa si alguien pasa null como argumento?

`calculator.xProjection(null, new Point(12, 13));` Por supuesto que obtendremos un NullPointerException. ¿Cómo lo resolvemos? Pudiéramos crear una nueva excepción y arrojarla:

```
public class MetricsCalculator {
	public double xProjection(Point p1, Point p2) {
		if (p1 == null || p2 == null) {
			throw InvalidArgumentException("Invalid argument for MetricsCalculator.xProjection");
		}
		return (p2.x − p1.x) * 1.5;
	}
}
```

¿Es mejor? Pudiera ser un poco mejor que un `NullPointerException`, pero recuerda, debemos definir un manejador para `InvalidArgumentException`...

En la mayoría de los lenguajes de programación no hay una buena manera para tratar con parámetros null que son pasados accidentalmente. Debido a esto, el enfoque racional es prohibir pasar null por defecto. Cuando hacemos esto, puedes programar con el conocimiento de que un null en un argumento indica que existe un problema. Como resultado se tienen muchos menos errores por descuido.

### Fallar rápidamente.
Las excepciones deben ser arrojadas tan pronto como sea posible después de haber sido detectado un caso excepcional. Esto ayuda a determinar la ubicación exacta del problema al observar el stack trace de la excepción. Por ejemplo:

```
public void checkOut() {
	...
	Item selectedItem = this.getSelectedItem(operationType);
	if (selectedItem == null) {
		Notifier.notifyError();
	} else {
		...
	}
}

public Item getSelectedItem(int operationType) {
	if (...) {
		return null;
	} else if (...) {
		return null;
	} else {
		...
	}
	return item;
}

public static void notifyError() {
	...
	throw new ItemNotFoundException("Item does not exist or is out of stock");
}
```

En el método getSelectedItem existen varios puntos donde existe un caso excepcional y no se está encontrando el artículo deseado. Sin embargo, existe sólo un punto donde se notifica que ocurrió un error, mismo que hace más difícil saber cuál fue la causa raíz de la excepción. En tal punto, donde se arroja la excepción, no es posible saber si fue por que no cumplió la condición del if o la condición del else if o cualquier otra condición que indique que no se ha encontrado el artículo deseado. Es mejor arrojar la excepción en el punto donde se está originando y no arrojarla hasta mucho después.

### No tragarse las excepciones.
Cada vez que una excepción es tragada, el sistema se queda en un estado inconsistente pudiendo generar más y más problemas. Por ejemplo:

```
public boolean persist(Employee e) {
	...
	try {
		if (1 / (e.getA() + e.getB()) {
			...
		}
		persistenceManager.save(e);
	} catch (Exception e) {
		System.err.println(e.getMessage());
	}
	return true ;
}
```

Este método persiste la entidad empleado si cumple con la condición indicada en el `if`, siempre y cuando `e.getA() + e.getB()` sea mayor que 0 y que no exista ninguna excepción al persistir en el recurso (Base de datos). En tales casos la excepción se manda a la consola pero se indica al invocador que el empleado fue persistido correctamente lo cual es falso.

Existen excepciones a la regla donde las excepciones pueden ser tragadas. Esto es siempre y cuando el caso excepcional está completamente resuelto en el bloque catch y no deja en un estado inconsistente a la aplicación.
