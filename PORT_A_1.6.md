# Supernatural Weapons — port a RimWorld 1.6

Port del mod **Supernatural Weapons** de **Artemiish** a RimWorld 1.6.
Anade dos armas anómalas: **The Revolver**, que mata de un solo disparo, y
**Mark of the Killer**, una maldición que crea un supersoldado inmortal a cambio
de un precio.

El original de este mod no se ha tocado: este repositorio es una copia de trabajo.
tocado**. Este port es una copia de trabajo.

## Estado

- **Carga con cero errores y cero avisos** en `Player.log`.
- Sus **10 definiciones** cargan sin un solo campo rechazado por 1.6.
- Sus **16 texturas** cargan (antes daban 13 avisos de textura no encontrada).
- **La DLL compilada para 1.5 funciona tal cual en 1.6**, sin errores de tipo ni
  de miembro. Era el riesgo principal del port y esta descartado.
- Pendiente: la comprobacion visual de los dos objetos en partida.

## Como estaba el mod

El autor guardaba el contenido repartido:

    Supernatural Weapons/
       1.5/          Defs (10 XML) y Assemblies (la DLL)
       About/        metadatos
       Languages/    traducciones
       Textures/     16 imagenes

Sin `LoadFolders.xml`, RimWorld carga **todo**, y por eso funcionaba en 1.5. Al
declarar el soporte de 1.6 hay que decirle **que carpetas** corresponden a cada
version, y ahi estaba el detalle que costo una prueba: el contenido NO esta todo
en la misma carpeta.

## Cambios hechos

| Cambio | Motivo |
|---|---|
| `About.xml`: se declara `<li>1.6</li>` | Para que el juego lo acepte como compatible |
| `LoadFolders.xml` nuevo | 1.6 debe cargar **la raiz** (Textures, Languages) **y `1.5`** (Defs, Assemblies). Mapearlo solo a `1.5` dejaba fuera las texturas y el juego avisaba 13 veces |
| `<causesNeed>` eliminado de `Knife.xml` | Campo que **1.6 ya no tiene** en `HediffDef`. No se pierde nada: dos lineas mas abajo el propio mod engancha la misma necesidad con su comp `HediffCompProperties_AttachedNeed` |
| `CompProperties_Styleable` anadido a `Artemiish_SNRevolverBase` | El juego avisaba de un relic de Ideology sin ese componente. Se anade en la base para que lo hereden todas las variantes |

**Nada eliminado**: el `<causesNeed>` quitado queda explicado en un comentario, y
los archivos apartados se renombran, nunca se borran.

## Dependencias

Las tres que declara el mod, y como se resolvieron:

| Dependencia | Como |
|---|---|
| **Anomaly** (DLC) | Ya estaba activo |
| **Vanilla Expanded Framework** | Aporta `MVCF.dll`, que el mod usa para sus verbos |
| **EBSG Framework** | Aporta las necesidades y los pensamientos del mod |

Las dos declaran compatibilidad con 1.6 en sus propios metadatos, y se comprobo
que las cuatro clases externas que el mod necesita siguen existiendo en sus DLL.


## Como se verifico

Ademas de leer `Player.log`, se valido **estaticamente** cada campo de los 10 XML
contra los tipos reales de 1.6, con un inspector que carga el `Assembly-CSharp` y
lista los campos de un tipo **incluyendo los heredados**:

    defs revisados: 20 | campos revisados: 168 | campos que 1.6 no reconoce: 0

Ese validador encontro el `<causesNeed>` **antes** de entrar al juego, y es la
primera vez en este proyecto que un error de XML se detecta sin gastar una prueba.
Queda guardado en `Herramientas\InspectorApi` (modo `!TipoDef`).

## Creditos

**Supernatural Weapons** es obra de **Artemiish**. Los creditos y la autoria se
conservan intactos en `About.xml`. Las dependencias son de sus respectivos autores
(Vanilla Expanded / Oskar Potocki, y el autor de EBSG Framework).
