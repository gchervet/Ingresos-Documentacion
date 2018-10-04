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
- Agregar en dicha clase un nuevo **Dictionary**:

```c#
this.Add(TiposCarrerasCodes.DIPLOMATURA, "Diplomatura");
```

- Buscar la clase **TiposCarrerasCodes** en **GGI.Ingreso.UI.Silverlight.Common.Enums**, donde se definen los códigos string que manejan los tipos de carrera.
- Agregar en dicha clase un nuevo **código string**:

```c#
public const string DIPLOMATURA = "diplomatura";
```

En ingresos no deberíamos agregar nada más, ya que dichos diccionarios se populan mediante la dll de Académico.

**En el servicio del Académico**:

Observar que se utiliza la dll correspondiente al **Domain.Facade** del **Academico**. Para modificarla deberemos abrir dicho proyecto desde el **visualizador de código fuente** de **VS 2008** y buscar el **método GetCarreras** de la clase **AcademicoFacade**: 

```c#
private Dictionary<string, Dictionary<string, string>> GetCarreras(string modalidad)
```
Observar que ni bien comienza el método, se generan los diccionarios que se devolverán luego al cliente de **Ingresos** (ver anotaciones en el código):

```c#
Dictionary<string, Dictionary<string, string>> mCarreras = new Dictionary<string, Dictionary<string, string>>();
            Dictionary<string, string> mGrado = new Dictionary<string, string>();
            Dictionary<string, string> mPos = new Dictionary<string, string>();
            Dictionary<string, string> mPre = new Dictionary<string, string>();
            Dictionary<string, string> mExtension = new Dictionary<string, string>();
            // Agregar nuevo diccionario aquí
            Dictionary<string, string> mDip = new Dictionary<string, string>();
```
Una vez agregado nuestro nuevo diccionario (en este caso **mDip**
 para Diplomaturas), la información de la base se obtendrá de la tabla **uniEscuelas** (esta tabla maneja las carreras, por más que se llame "Escuelas"). El sistema verifica mediante el campo **Nivel** de dicha tabla para ver en qué diccionario agregar cada carrera:

```c#
if (mEscuela.Nivel.ToLower() == "grado")
{
      mGrado.Add(mEscuela.Id, mNombre);
}
else
{
	if (mEscuela.Nivel.ToLower() == "posgrado")
		mPos.Add(mEscuela.Id, mNombre);
	else if(mEscuela.Nivel.ToLower() == "diplomatura")
		mDip.Add(mEscuela.Id, mNombre);
	else // Ante cualquier discordancia, se agrega la carrera a Pregrado
		mPre.Add(mEscuela.Id, mNombre);
}
```

Lo que se debe hacer es agregar un nuevo **If** para verificar el nuevo nombre, en este caso se agregaron las siguientes líneas:

```c#
	else if(mEscuela.Nivel.ToLower() == "diplomatura")
		mDip.Add(mEscuela.Id, mNombre);
```
