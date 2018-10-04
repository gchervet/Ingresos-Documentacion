# Ingresos - Documentación

![](https://raw.githubusercontent.com/gchervet/Documentacion/master/images/kennedy_logo.jpg)

**Índice**

* [1. Ubicaciones y dónde modificar](#ubicaciones)
* [2. Agregar un tipo de carrera](#agregartipocarrera)

---------------------------------------

<a name="ubicaciones" />

# 1. Ubicaciones y dónde modificar

Ingresos 

<a name="agregartipocarrera" />

# 2. Agregar un tipo de carrera

Para modificar, agregar o eliminar un tipo de carrera hay que tener en cuenta lo siguiente:

	1. Los tipos de carrera se cargan mediante un Dictionary y NO ESTÁN EN LA BASE.
	2. Las carreras en sí se buscan en la tabla uniEscuelas, y el campo Nivel las diferencia.

Las DLL que utiliza **Ingresos** para realizar dichas tareas son:

	GGI.Academico.Domain.Facade.dll
		En C:\cs\DEV2\GGI\GGI.Academico\GGI.Academico.Domain.Facade\bin\Debug
	
	GGI.Ingreso.UI.Silverlight.App.xap
		En C:\cs\DEV2\GGI\GGI.Ingreso\GGI.Ingreso.UI.Silverlight.App.Web\ClientBin

**En el cliente Silverlight:**

	C:\cs\DEV2\GGI\GGI.Ingreso\GGI.Ingreso.UI.Silverlight.App.Web

- Buscar la clase **TiposCarrerasDictionary** en **GGI.Ingreso.UI.Silverlight.Common.Dictionaries**, donde se definen los tipos de carrera mediante un **string** que se almacenará en un **Dictionary**.
- Agregar en dicha clase un nuevo **string**:
		this.Add(TiposCarrerasCodes.DIPLOMATURA, "Diplomatura");
- Buscar la clase **TiposCarrerasCodes** en **GGI.Ingreso.UI.Silverlight.Common.Enums**, donde se definen los enum que manejan los tipos de carrera.
- Agregar en dicha clase un nuevo **enum**:



Observar que se utiliza la dll correspondiente al **Domain.Facade** del **Academico**. Para modificarla deberemos abrir dicho proyecto desde el **visualizador de código fuente** de **VS 2008** y buscar el **método GetCarreras** de la clase **AcademicoFacade**: 

```c#
private Dictionary<string, Dictionary<string, string>> GetCarreras(string modalidad)
```

