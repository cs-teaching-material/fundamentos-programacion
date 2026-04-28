# Guia docente - Sesion 19: Esqueleto modular y alcance de variables

**Curso:** Logica de Programacion  
**Grupos:** 810 / 811  
**Saberes de unidad abordados:** Alcance avanzado, estado compartido minimo, shadowing, esqueleto modular por capas

---

## Objetivo de la sesion

Que el estudiante comprenda y aplique reglas de alcance en un esqueleto modular robusto, desarrollando junto al profesor un ejemplo completo en C# sin actividades evaluativas.

---

## Slide a slide - orientaciones

### Slide 1 - Portada
- Abrir con el cambio de foco: esta sesion no evalua, se desarrolla en vivo.

### Slide 2 - Proposito
- Validar que el grupo entienda diferencia entre "separar funciones" y "gobernar alcance".

### Slide 3 - Planeacion U3
**Pregunta de activacion:** Que agrega la sesion 19 frente a la 18?  
**Respuesta esperada:** Control de estado, sombra de variables y decisiones de alcance.

### Slide 4 - Matriz de alcance
- Tomar 2 decisiones de la tabla y pedir ejemplos al grupo.

### Slide 5 - Diagrama
- Marcar visualmente alcance de clase, funcion y bloque.

### Slide 6 - Shadowing
- Explicar por que un nombre repetido puede ocultar un bug.
- Cerrar diciendo explicitamente: el problema no es "tener un campo", sino ocultarlo o depender de el sin regla.

### Slide 7 - Estado compartido
- Entrar desde la transicion anterior: una vez evitada la sombra, recien se decide si el campo compartido merece existir.
- Reforzar regla: estado compartido minimo y justificado.

### Slide 8 - Analogia visual
- Usar la comparacion "tablero comun vs cuaderno personal" para fijar intuicion.
- Preguntar que datos del ejemplo pertenecen a cada lado.

### Slide 9 - Arquitectura por capas
- Mostrar secuencia exacta de llamadas del modulo.

### Slide 10 - Contratos
- Pedir una precondicion y una invariante por funcion critica.

### Slide 11 - value/ref/out
- Resolver 3 microcasos de eleccion de mecanismo.

### Slide 12 - Plantilla base
- Montar estructura general del programa en vivo.

### Slide 13 - Flujo animado
- Recorrer paso a paso el ciclo de datos.

### Slide 14 - Diagnostico
- Identificar fallas de alcance en codigo defectuoso.

### Slide 15 - Refactor guiado
- Corregir fallas sin introducir campos innecesarios.

### Slide 16 - Taller guiado
- Presentar el reto tecnico del desarrollo en clase (no evaluativo).

### Slide 17 - Desarrollo paso 1
- Construir `Main` y el flujo general.

### Slide 18 - Desarrollo paso 2
- Implementar entrada y validacion con `out`.

### Slide 19 - Desarrollo paso 3
- Implementar dominio, auditoria y salida.

### Slide 20 - Ejemplo completo
- Desarrollar y explicar el codigo completo de principio a fin.

### Slide 21 - Pruebas en vivo
- Ejecutar al menos 3 casos y analizar salida.

### Slide 22 - Depuracion guiada
- Revisar alcance real con breakpoints.

### Slide 23 - Extension opcional
- Si hay tiempo, agregar bonificacion o eventos extra de auditoria.

### Slide 24 - Errores comunes
- Cerrar lista de anti patrones de alcance.

### Slide 25 - Sintesis tecnica
- Consolidar reglas no negociables de arquitectura.

### Slide 26 - Cierre
- Enlazar con sesion 20: implementacion completa.

---

## Taller guiado en clase - respuestas esperadas

### Enunciado
Construir, junto al profesor, un modulo de nomina semanal con entrada validada, calculo de neto, descuento de salud, salida final y auditoria con contador de llamadas.

### Solucion de referencia (C#)

```csharp
using System;

class Program
{
    static int contadorLlamadas = 0;

    static void Main()
    {
        if (!LeerEntradaNomina(out double horas, out double tarifa))
        {
            return;
        }

        double neto = CalcularNeto(horas, tarifa);
        double descuento = CalcularDescuentoSalud(neto);
        double pagoFinal = CalcularPagoFinal(neto, descuento);

        RegistrarAuditoria("nomina-semanal");
        MostrarBoleta(horas, tarifa, neto, descuento, pagoFinal);
    }

    static bool LeerEntradaNomina(out double horas, out double tarifa)
    {
        horas = 0;
        tarifa = 0;

        Console.Write("Horas trabajadas: ");
        if (!double.TryParse(Console.ReadLine(), out horas) || horas <= 0)
        {
            Console.WriteLine("Horas invalidas");
            return false;
        }

        Console.Write("Tarifa por hora: ");
        if (!double.TryParse(Console.ReadLine(), out tarifa) || tarifa <= 0)
        {
            Console.WriteLine("Tarifa invalida");
            return false;
        }

        return true;
    }

    static double CalcularNeto(double horas, double tarifa)
    {
        return horas * tarifa;
    }

    static double CalcularDescuentoSalud(double neto)
    {
        return neto * 0.04;
    }

    static double CalcularPagoFinal(double neto, double descuento)
    {
        return neto - descuento;
    }

    static void RegistrarAuditoria(string evento)
    {
        contadorLlamadas++;
        Console.WriteLine($"[AUDIT] {contadorLlamadas}: {evento}");
    }

    static void MostrarBoleta(double horas, double tarifa, double neto, double descuento, double pagoFinal)
    {
        Console.WriteLine("--- Boleta semanal ---");
        Console.WriteLine($"Horas: {horas}");
        Console.WriteLine($"Tarifa: {tarifa}");
        Console.WriteLine($"Neto: {neto}");
        Console.WriteLine($"Descuento salud: {descuento}");
        Console.WriteLine($"Pago final: {pagoFinal}");
    }
}
```

### Casos de prueba sugeridos

| Entrada | Salida esperada |
|---------|-----------------|
| horas=10, tarifa=15000 | neto=150000, descuento=6000, pago final=144000 |
| horas=8, tarifa=20000 | neto=160000, descuento=6400, pago final=153600 |
| horas=-2, tarifa=15000 | mensaje de horas invalidas y finaliza |

### Errores frecuentes a anticipar

| Error tipico | Correccion |
|-------------|------------|
| Sombra de variable local sobre campo | Renombrar variable local y limitar campos compartidos |
| Guardar entrada de usuario en campos globales | Pasar datos por parametros y retornos |
| Recalcular dominio en salida | Mantener salida solo para impresion |
| No validar entrada | Bloquear flujo cuando lectura no es valida |

---

## Formato de evidencia esperada

**Plataforma:** GitHub (repositorio personal del estudiante)  
**Ruta:** `sesion19/` en la rama `main`

### Estructura minima

```text
sesion19/
  |-- Programa.cs
  |-- sesion19.csproj
  |-- README.md
```

### Checklist docente

- [ ] El programa compila con `dotnet run`.
- [ ] El flujo separa entrada, dominio, auditoria y salida.
- [ ] No hay sombras de variables sin justificar.
- [ ] Solo existe estado compartido minimo y controlado.
- [ ] Los casos de prueba en vivo coinciden con resultados esperados.

---

## Recursos de apoyo para el docente

- Documentacion oficial C# / .NET: <https://learn.microsoft.com/es-es/dotnet/csharp/>
- Alcance y lifetime en C#: <https://learn.microsoft.com/es-es/dotnet/csharp/language-reference/language-specification/variables>
- Extension C# para VS Code: ms-dotnettools.csharp
