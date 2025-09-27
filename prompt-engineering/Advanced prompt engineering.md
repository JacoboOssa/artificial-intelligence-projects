# Advanced prompt engineering

# **Ejercicio 1: Generación de una Función de Código**

---

## **1.A: Prompt Simple**

```python
import re

def validar_contraseña(password, longitud_minima=8, longitud_maxima=128):
    """
    Valida una contraseña según criterios de seguridad.
    
    Args:
        password (str): La contraseña a validar
        longitud_minima (int): Longitud mínima requerida (por defecto 8)
        longitud_maxima (int): Longitud máxima permitida (por defecto 128)
    
    Returns:
        tuple: (bool, list) - (es_valida, lista_de_errores)
    """
    errores = []
    
    # Verificar longitud
    if len(password) < longitud_minima:
        errores.append(f"La contraseña debe tener al menos {longitud_minima} caracteres")
    
    if len(password) > longitud_maxima:
        errores.append(f"La contraseña no puede tener más de {longitud_maxima} caracteres")
    
    # Verificar que contenga al menos una letra minúscula
    if not re.search(r'[a-z]', password):
        errores.append("La contraseña debe contener al menos una letra minúscula")
    
    # Verificar que contenga al menos una letra mayúscula
    if not re.search(r'[A-Z]', password):
        errores.append("La contraseña debe contener al menos una letra mayúscula")
    
    # Verificar que contenga al menos un dígito
    if not re.search(r'[0-9]', password):
        errores.append("La contraseña debe contener al menos un número")
    
    # Verificar que contenga al menos un carácter especial
    if not re.search(r'[!@#$%^&*()_+\-=\[\]{};:"\\|,.<>\/?]', password):
        errores.append("La contraseña debe contener al menos un carácter especial (!@#$%^&*()_+-=[]{}|;:,.<>?)")
    
    # Verificar que no tenga espacios en blanco
    if ' ' in password:
        errores.append("La contraseña no debe contener espacios en blanco")
    
    # Verificar caracteres repetidos consecutivos (máximo 2)
    if re.search(r'(.)\1{2,}', password):
        errores.append("La contraseña no debe tener más de 2 caracteres idénticos consecutivos")
    
    return len(errores) == 0, errores

def validar_contraseña_simple(password, longitud_minima=6):
    """
    Versión simplificada del validador de contraseñas.
    
    Args:
        password (str): La contraseña a validar
        longitud_minima (int): Longitud mínima requerida (por defecto 6)
    
    Returns:
        bool: True si la contraseña es válida, False en caso contrario
    """
    return (len(password) >= longitud_minima and 
            any(c.islower() for c in password) and
            any(c.isupper() for c in password) and
            any(c.isdigit() for c in password))

def generar_reporte_seguridad(password):
    """
    Genera un reporte de seguridad detallado de la contraseña.
    
    Args:
        password (str): La contraseña a evaluar
    
    Returns:
        dict: Diccionario con el análisis de seguridad
    """
    reporte = {
        'longitud': len(password),
        'tiene_minusculas': bool(re.search(r'[a-z]', password)),
        'tiene_mayusculas': bool(re.search(r'[A-Z]', password)),
        'tiene_numeros': bool(re.search(r'[0-9]', password)),
        'tiene_especiales': bool(re.search(r'[!@#$%^&*()_+\-=\[\]{};:"\\|,.<>\/?]', password)),
        'sin_espacios': ' ' not in password,
        'sin_repeticiones_excesivas': not bool(re.search(r'(.)\1{2,}', password))
    }
    
    # Calcular puntuación de seguridad
    puntuacion = 0
    if reporte['longitud'] >= 8:
        puntuacion += 20
    if reporte['longitud'] >= 12:
        puntuacion += 10
    if reporte['tiene_minusculas']:
        puntuacion += 15
    if reporte['tiene_mayusculas']:
        puntuacion += 15
    if reporte['tiene_numeros']:
        puntuacion += 15
    if reporte['tiene_especiales']:
        puntuacion += 20
    if reporte['sin_espacios']:
        puntuacion += 5
    
    reporte['puntuacion'] = min(puntuacion, 100)
    
    if puntuacion >= 80:
        reporte['nivel_seguridad'] = 'Muy Fuerte'
    elif puntuacion >= 60:
        reporte['nivel_seguridad'] = 'Fuerte'
    elif puntuacion >= 40:
        reporte['nivel_seguridad'] = 'Moderada'
    else:
        reporte['nivel_seguridad'] = 'Débil'
    
    return reporte

# Ejemplos de uso
if __name__ == "__main__":
    # Ejemplos de contraseñas para probar
    contraseñas_ejemplo = [
        "123456",
        "password",
        "Password123",
        "MiContraseña2024!",
        "Abc123!@#",
        "ContraseñaSuperSegura2024!",
        "aaa111AAA"
    ]
    
    print("=== VALIDADOR DE CONTRASEÑAS ===\n")
    
    for password in contraseñas_ejemplo:
        print(f"Contraseña: '{password}'")
        es_valida, errores = validar_contraseña(password)
        
        if es_valida:
            print("✅ Contraseña VÁLIDA")
        else:
            print("❌ Contraseña INVÁLIDA")
            for error in errores:
                print(f"   • {error}")
        
        # Mostrar reporte de seguridad
        reporte = generar_reporte_seguridad(password)
        print(f"   Nivel de seguridad: {reporte['nivel_seguridad']} ({reporte['puntuacion']}/100)")
        print("-" * 50)
```

---

## Link respuesta claude

https://claude.ai/share/fcd3635b-33f8-4881-8634-68c182e00905

---

## **1.B: Prompt Avanzado**

```python
def validar_contrasena(contrasena):
    """
    Valida si una contraseña cumple con los criterios de seguridad establecidos.
    
    Esta función verifica que la contraseña cumpla con todos los siguientes criterios:
    - Longitud mínima de 12 caracteres
    - Al menos una letra mayúscula (A-Z)
    - Al menos una letra minúscula (a-z)
    - Al menos un número (0-9)
    - Al menos un carácter especial (@, #, $, %, &, !)
    
    Args:
        contrasena (str): La contraseña a validar
        
    Returns:
        bool: True si la contraseña cumple todos los criterios, False en caso contrario
        
    Examples:
        >>> validar_contrasena("MiPassword123!")
        True
        >>> validar_contrasena("password123")
        False
        >>> validar_contrasena("PASSWORD123!")
        False
    """
    # Verificar longitud mínima
    if len(contrasena) < 12:
        return False
    
    # Definir caracteres especiales permitidos
    caracteres_especiales = "@#$%&!"
    
    # Inicializar flags para cada criterio
    tiene_mayuscula = False
    tiene_minuscula = False
    tiene_numero = False
    tiene_especial = False
    
    # Verificar cada carácter de la contraseña
    for caracter in contrasena:
        if caracter.isupper():
            tiene_mayuscula = True
        elif caracter.islower():
            tiene_minuscula = True
        elif caracter.isdigit():
            tiene_numero = True
        elif caracter in caracteres_especiales:
            tiene_especial = True
    
    # Retornar True solo si todos los criterios se cumplen
    return tiene_mayuscula and tiene_minuscula and tiene_numero and tiene_especial

# Ejemplos de uso
if __name__ == "__main__":
    print("=== Ejemplos de validación de contraseñas ===\n")
    
    # Ejemplo 1: Contraseña válida
    contrasena_valida = "MiPassword123!"
    resultado = validar_contrasena(contrasena_valida)
    print(f"Contraseña: '{contrasena_valida}'")
    print(f"Resultado: {resultado}")
    print(f"Explicación: {'✅ Válida - Cumple todos los criterios' if resultado else '❌ Inválida'}\n")
    
    # Ejemplo 2: Contraseña inválida - Falta mayúscula y carácter especial
    contrasena_invalida1 = "mipassword123"
    resultado1 = validar_contrasena(contrasena_invalida1)
    print(f"Contraseña: '{contrasena_invalida1}'")
    print(f"Resultado: {resultado1}")
    print(f"Explicación: {'✅ Válida' if resultado1 else '❌ Inválida - Falta mayúscula y carácter especial'}\n")
    
    # Ejemplo 3: Contraseña inválida - Muy corta
    contrasena_invalida2 = "Pass123!"
    resultado2 = validar_contrasena(contrasena_invalida2)
    print(f"Contraseña: '{contrasena_invalida2}'")
    print(f"Resultado: {resultado2}")
    print(f"Explicación: {'✅ Válida' if resultado2 else '❌ Inválida - Muy corta (menos de 12 caracteres)'}")
```

---

## Link respuesta claude

https://claude.ai/share/47b67e4d-c637-4285-8d1d-81c45399d1aa

---

## **1.C: Evaluación Comparativa**

1. **Completitud:** ¿La función del prompt simple incluyó todas las reglas?
    
    El prompt simple es mucho más completo, ya que incluye múltiples reglas de validación (longitud mínima y máxima, mayúsculas, minúsculas, dígitos, caracteres especiales, ausencia de espacios y control de repeticiones consecutivas). Sin embargo, esta amplitud también puede volverse excesiva o “alucinada” en algunos contextos, agregando validaciones que quizá no sean necesarias según los requerimientos de la aplicación. En contraste, el prompt avanzado es más acotado y directo, pues se centra en un conjunto específico de condiciones que pueden ajustarse mejor a necesidades concretas de seguridad, evitando una validación demasiado estricta o sobrecargada.
    
2. **Claridad:** ¿El código está bien documentado? ¿Es fácil de entender?
    
    El prompt avanzado es más claro que el simple, debido a que presenta un docstring conciso y ejemplos de uso que explican directamente qué esperar en cada caso. El código resulta más fácil de entender para alguien que lo lee por primera vez, ya que se enfoca en lo esencial y mantiene la lógica simple. En cambio, el prompt simple, aunque bien documentado, es más extenso y con múltiples validaciones que pueden hacerlo menos inmediato para comprender.
    
3. **Utilidad:** ¿El prompt avanzado proporcionó ejemplos de uso que facilitan la prueba y la integración?
    
    Ambos prompts son útiles, pero lo son de maneras diferentes. El prompt simple resulta muy valioso para sistemas en producción, ya que genera reportes detallados con puntuaciones y niveles de seguridad, lo que permite no solo validar sino también evaluar la calidad de la contraseña. El prompt avanzado, en cambio, ofrece ejemplos prácticos y explicaciones claras en la salida, lo que facilita realizar pruebas rápidas e integrar la función en un entorno de aprendizaje o demostración.
    
4. **Robustez:** ¿La función maneja casos borde que el prompt simple podría haber ignorado?
    
    El prompt simple es más robusto porque maneja de forma explícita varios casos borde que podrían comprometer la seguridad, como la detección de espacios en blanco, la prohibición de repeticiones excesivas y la validación de un rango de longitud más amplio. Por su parte, el prompt avanzado no contempla estos escenarios, ya que se limita a un conjunto fijo de requisitos y omite situaciones que podrían hacer que una contraseña sea débil en la práctica, a pesar de cumplir los criterios definidos.
    

---

# **Ejercicio 2: Extracción de Información a Formato JSON**

---

## **2.A: Prompt Simple**

```python
{
  "proyecto": {
    "nombre": "Proyecto Fénix",
    "reunion_lanzamiento": {
      "fecha": "2024-10-15",
      "hora": "10:30",
      "ubicacion": "Sala Júpiter"
    },
    "equipo": [
      {
        "nombre": "Ana García",
        "rol": "Líder de Proyecto"
      },
      {
        "nombre": "Carlos Rodríguez",
        "rol": "Desarrollador Principal"
      },
      {
        "nombre": "Elena Fernández",
        "rol": "Diseñadora UX"
      }
    ],
    "presupuesto": {
      "monto": 25000,
      "moneda": "USD"
    }
  }
}
```

---

## Link respuesta claude

https://claude.ai/share/12b79ae9-252a-46bf-980e-6f8131b5f5fa

---

## **2.B: Prompt Avanzado**

```python
{
  "proyecto": {
    "nombre": "Proyecto Fénix",
    "presupuesto": {
      "monto": 25000,
      "moneda": "USD"
    }
  },
  "reunion": {
    "fecha": "2024-10-15",
    "hora": "10:30",
    "ubicacion": "Sala Júpiter"
  },
  "asistentes": [
    {
      "nombre": "Ana García",
      "rol": "Líder de Proyecto"
    },
    {
      "nombre": "Carlos Rodríguez",
      "rol": "Desarrollador Principal"
    },
    {
      "nombre": "Elena Fernández",
      "rol": "Diseñadora UX"
    }
  ]
}
```

---

## Link respuesta claude

https://claude.ai/share/a6fc60ef-711e-4935-8d51-438dc1816ffe

---

## **2.C: Evaluación Comparativa**

1. **Estructura:** ¿El JSON del prompt simple tiene una estructura lógica y predecible?
    
    El simple es más anidado y lógico si todo pertenece al proyecto, pero menos flexible. El avanzado es modular y facilita acceder a cada bloque.
    
2. **Precisión:** ¿Los tipos de datos son correctos (números vs. strings)?
    
    Ambos usan bien los tipos: números para montos, strings para texto, fechas y horas.
    
3. **Parseabilidad:** ¿El JSON generado por el prompt avanzado es directamente utilizable por una aplicación sin necesidad de limpieza o conversión?
    
    El avanzado es más fácil de parsear porque separa módulos. El simple requiere navegar más niveles.
    
4. **Manejo de Nulos:** ¿Cómo manejaría cada uno un texto donde, por ejemplo, el presupuesto no se menciona?
    
    En el simple, si falta presupuesto, hay que validar dentro de "proyecto". En el avanzado, al estar separado, es más fácil detectar y manejar la ausencia.
    

---

# Ejercicio 3: Escritura Creativa con Tono y Estilo

---

## **3.A: Prompt Simple**

Los agujeros negros son objetos cósmicos fascinantes que representan una de las manifestaciones más extremas de la gravedad en el universo. Se forman cuando una estrella masiva, típicamente con más de 20-25 veces la masa de nuestro Sol, llega al final de su vida y colapsa bajo su propia gravedad de manera tan intensa que nada, ni siquiera la luz, puede escapar de su atracción gravitacional.

**Características fundamentales**

El elemento más distintivo de un agujero negro es el horizonte de eventos, una frontera invisible que marca el punto de no retorno. Una vez que algo cruza esta barrera, queda atrapado para siempre. El radio del horizonte de eventos, conocido como radio de Schwarzschild, depende directamente de la masa del agujero negro: a mayor masa, mayor tamaño.

En el centro de un agujero negro se encuentra la singularidad, un punto teórico donde la densidad se vuelve infinita y las leyes de la física tal como las conocemos dejan de aplicarse. Aquí, el espacio-tiempo se curva de manera extrema, creando condiciones que aún no comprendemos completamente.

**Tipos de agujeros negros**

Existen principalmente tres categorías. Los agujeros negros estelares, que se forman del colapso de estrellas individuales y tienen masas de unas pocas a decenas de veces la masa solar. Los agujeros negros supermasivos, que pueden contener millones o miles de millones de masas solares y residen en los centros de las galaxias. Y los agujeros negros de masa intermedia, una categoría menos común que los científicos aún están estudiando.

**Efectos sobre el espacio-tiempo**

Los agujeros negros distorsionan profundamente el espacio-tiempo a su alrededor. Esta curvatura extrema produce efectos relativistas notables, como la dilatación del tiempo: el tiempo transcurre más lentamente cerca del horizonte de eventos comparado con regiones más alejadas. También pueden actuar como lentes gravitacionales, curvando la luz de objetos distantes y permitiendo observar galaxias que de otra manera estarían ocultas.

**Radiación de Hawking**

Stephen Hawking propuso que los agujeros negros no son completamente negros, sino que emiten una radiación térmica muy débil debido a efectos cuánticos cerca del horizonte de eventos. Esta radiación es extremadamente tenue para agujeros negros de masa estelar, pero sugiere que eventualmente se evaporarían, aunque en escalas de tiempo incomprensiblemente largas.

**Observación y detección**

Aunque son invisibles por definición, los agujeros negros se pueden detectar por sus efectos gravitacionales en la materia circundante. Cuando material cae hacia un agujero negro, se acelera y calienta, formando un disco de acreción que brilla intensamente antes de desaparecer más allá del horizonte de eventos. En 2019, el Event Horizon Telescope logró capturar la primera imagen directa de la sombra de un agujero negro supermasivo en la galaxia M87.

**Importancia cosmológica**

Los agujeros negros desempeñan roles cruciales en la evolución galáctica. Los supermasivos en los centros de las galaxias pueden regular la formación estelar a través de jets de material y radiación que expulsan al espacio. También son laboratorios naturales para estudiar la física en condiciones extremas que no podemos replicar en la Tierra.

**Misterios pendientes**

Muchas preguntas sobre los agujeros negros siguen sin respuesta. La paradoja de la información cuestiona qué sucede con la información que cae en un agujero negro cuando este se evapora. La naturaleza exacta de las singularidades y si realmente existen sigue siendo un enigma que requiere una teoría unificada de la gravedad cuántica para resolverse.

Los agujeros negros continúan siendo objetos de intensa investigación, combinando aspectos de la relatividad general, la mecánica cuántica y la astrofísica en formas que siguen desafiando nuestra comprensión del universo.

---

## Link respuesta claude

https://claude.ai/share/c7795076-d56f-4e33-bcef-5b0244ecbe46

---

## **3.B: Prompt Avanzado**

Imaginen el objeto más extraño y fascinante del universo: un agujero negro es como una aspiradora cósmica tan poderosa que puede tragarse estrellas enteras, planetas, e incluso la luz misma. Si pudiéramos viajar cerca de uno, veríamos cómo todo a su alrededor gira en una danza espiral, como el agua que se va por el desagüe de la bañera, pero millones de veces más brillante y colorido. Lo más asombroso es que estos gigantes invisibles moldean galaxias enteras con su fuerza, actuando como maestros arquitectos del cosmos. Aunque no podemos verlos directamente porque ni siquiera la luz puede escapar de ellos, los científicos los detectan observando cómo las estrellas bailan a su alrededor, revelándonos uno de los secretos más profundos del universo.

---

## Link respuesta claude

https://claude.ai/share/777e5c14-08b3-4e39-a3aa-bab39136c8c7

## **3.C: Evaluación Comparativa**

1. **Adecuación a la Audiencia:** ¿Qué respuesta es más apropiada y comprensible para un niño?
    
    El prompt avanzado es más apropiado para un niño porque usa comparaciones simples (aspiradora, desagüe, baile) y un lenguaje fácil de entender. El simple usa demasiada jerga técnica.
    
2. **Tono:** ¿Logró el prompt simple capturar el tono de asombro solicitado?
    
    El prompt simple es muy académico y no transmite asombro. El avanzado sí logra un tono de maravilla y curiosidad.
    
3. **Creatividad:** ¿Qué respuesta es más original y memorable?
    
    El prompt avanzado es más original y memorable gracias a las metáforas visuales y comparaciones cotidianas. El simple es más estándar y técnico.
    
4. **Cumplimiento de Restricciones:** ¿El prompt avanzado logró evitar la jerga técnica como se le pidió?
    
    El prompt avanzado evita la jerga técnica y mantiene un lenguaje accesible. El simple incluye términos complejos como “horizonte de eventos” o “singularidad” que no cumplen esa restricción.
    

---