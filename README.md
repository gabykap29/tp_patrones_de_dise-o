``` typescript
/* 
Ejercicio 1: Gestionar Configuración Global del Sistema
Objetivo: Implementar un patrón Singleton para gestionar la configuración global de la
aplicación de inventario.


● Crear una clase Configuracion que siga el patrón Singleton.
● Esta clase debe almacenar propiedades como idioma, rutaBaseDatos y nivelRegistro.
● Agregar métodos para obtener y actualizar cada una de estas propiedades.
● Asegurar que solo exista una instancia de Configuracion en toda la aplicación.

*/
interface IConfig{
    idioma: string;
    rutaBd: string;
    nivelRegistro: string;
}

class Config{
    private static instance: Config | null = null;

    private config: IConfig | null = null; 
    
    private constructor(){}

    public static getIntance():Config {
        if(Config.instance === null){
            Config.instance = new Config()
        }
        return Config.instance;
    }

    public addConfig(idioma: string, rutaBd: string, nivelRegistro: string): void {
        this.config = { idioma, rutaBd, nivelRegistro };
    }
    public getConfig():IConfig | null {
        return this.config;
    }

    public editConfig(idioma?:string, rutaBd?:string, nivelRegistro?:string):void{
        if(this.config){
            this.config.idioma = idioma ?? this.config.idioma;
            this.config.rutaBd =  rutaBd?? this.config.rutaBd;
            this.config.nivelRegistro =  nivelRegistro?? this.config.nivelRegistro;
        }else{
            console.log("No hay configuración que configurar");
        }

    }
}

// Ejemplo de uso:
const configInstance = Config.getIntance();

// Crear configuración inicial
configInstance.addConfig("español", "/ruta/base/datos", "info");

// Obtener configuración actual
console.log(configInstance.getConfig());  // { idioma: 'español', rutaBd: '/ruta/base/datos', nivelRegistro: 'info' }

// Editar configuración
configInstance.editConfig("inglés", undefined, "debug");

// Obtener configuración actualizada
console.log(configInstance.getConfig());

/*  
Ejercicio 2: Control de Conexiones a la Base de Datos
Objetivo: Utilizar el patrón Singleton para manejar la conexión a la base de datos de
inventario.
● Crear una clase ConexionDB que siga el patrón Singleton.
● Esta clase debe manejar la conexión a una base de datos ficticia, con propiedades
como host, puerto y usuario.
● Implementar métodos para conectar y desconectar la base de datos.
● Garantizar que todas las partes de la aplicación utilicen la misma instancia de
ConexionDB.

*/

interface IConection{
    host: string;
    port: number,
    user: string;
}

class Connection{
    private static instance: Connection | null = null;
    private conection: IConection | null = null;
    private constructor(){}
    public static getInstance():Connection{
        if(Connection.instance === null){
            Connection.instance = new Connection();
        }
        return Connection.instance;
    }
    public createConnection(host:string, port:number, user:string):void{
        this.conection = {host, port, user}
        console.log("Conexión creada!");
    }
    public connect(host:string, port:number, user:string):void{
        if(host === this.conection?.host && port === this.conection.port && user === this.conection.user){
            console.log(`Conectado a la base de datos en ${host}:${port}, con el usuario ${user}`);
        }else{
            console.log("No fue posible conectar a la base de datos!");
        }
        
    }
    public disconect():void{
        if(this.conection){
            console.log("Desconectado de la base de datos");
            this.conection === null;
        }else{
            console.log("No hay conexiones activas!");
        }
    }
    public isConnect():boolean{
        return this.conection ? true : false;
    }

}


// Ejemplo de uso:

const dbConnection = Connection.getInstance();

// Crear la conexión
dbConnection.createConnection("localhost", 5432, "admin");

// Intentar conectar
dbConnection.connect("localhost", 5432, "admin");  // Conexión exitosa

// Desconectar
dbConnection.disconect();  // Desconectado exitosamente

// Intentar desconectar de nuevo
dbConnection.disconect();  // No hay ninguna conexión activa


/*
Patrón Factory Method

Ejercicio 1: Crear Dispositivos de Entrada
Objetivo: Utilizar el patrón Factory Method para crear diferentes tipos de dispositivos de
entrada.
● Crear una clase DispositivoEntradaFactory con un método crearDispositivo que,
basado en el tipo de dispositivo ("Teclado", "Ratón", "Scanner"), devuelva una
instancia de la clase adecuada.
● Crear clases específicas para cada tipo de dispositivo (Teclado, Ratón, Scanner), cada
una con sus propias propiedades (Ej.: tipoConexion, marca).
● Integrar la creación de estos dispositivos en el sistema de inventario.

*/

interface IDispositivo{
    tipoConexion: string;
    marca: string;
    detalles():string;
}

class Teclado implements IDispositivo{
    tipoConexion: string;
    marca: string;
    constructor(tipoConexion: string, marca:string){
        this.tipoConexion = tipoConexion;
        this.marca = marca;
    }
    detalles(): string {
        return `Teclado marca ${this.marca}, con tipo de conexión: ${this.tipoConexion}`;
    }
}

class Mouse implements IDispositivo{
    tipoConexion: string;
    marca: string;
    constructor(tipoConexion: string, marca:string){
        this.marca = marca;
        this.tipoConexion = tipoConexion;
    }
    detalles(): string {
        return `Mouse marca ${this.marca}, con tipo de conexión: ${this.tipoConexion}`;
    }
}

class Scanner implements IDispositivo{
    tipoConexion: string;
    marca: string;
    constructor( tipoConexion:string, marca:string ){
        this.marca = marca;
        this.tipoConexion = tipoConexion;
    }
    detalles(): string {
        return `Scanner marca ${this.marca}, con tipo de conexión: ${this.tipoConexion}`;
    }
}

class DispositivoEntradaFactory{
    public static crearDispositivo(tipo: string, marca: string, tipoConexion: string){
        switch (tipo) {
            case "Teclado":
                return new Teclado( tipoConexion, marca);
            case "Mouse":
                return new Mouse( tipoConexion, marca);
            case "Scanner":
                return new Scanner( tipoConexion, marca);
            default:
                return "No hay metodo disponible para el dispositivo que quiere crear!"
        }
    }
}

// Función de prueba
function probarDispositivoFactory() {
    // Crear un Teclado
    const teclado = DispositivoEntradaFactory.crearDispositivo("Teclado", "Logitech", "USB");
    if (teclado instanceof Teclado) {
        console.log(teclado.detalles()); // Debería imprimir: Teclado marca Logitech, con tipo de conexión: USB
    } else {
        console.log("Error al crear Teclado");
    }

    // Crear un Mouse
    const mouse = DispositivoEntradaFactory.crearDispositivo("Mouse", "Razer", "Bluetooth");
    if (mouse instanceof Mouse) {
        console.log(mouse.detalles()); // Debería imprimir: Mouse marca Razer, con tipo de conexión: Bluetooth
    } else {
        console.log("Error al crear Mouse");
    }

    // Crear un Scanner
    const scanner = DispositivoEntradaFactory.crearDispositivo("Scanner", "Epson", "WiFi");
    if (scanner instanceof Scanner) {
        console.log(scanner.detalles()); // Debería imprimir: Scanner marca Epson, con tipo de conexión: WiFi
    } else {
        console.log("Error al crear Scanner");
    }

    // Intentar crear un dispositivo no soportado
    const dispositivoInvalido = DispositivoEntradaFactory.crearDispositivo("Impresora", "HP", "USB");
    if (typeof dispositivoInvalido === "string") {
        console.log(dispositivoInvalido); // Debería imprimir: No hay metodo disponible para el dispositivo que quiere crear!
    } else {
        console.log("Error: Se creó un dispositivo no válido.");
    }
}

// Ejecutar las pruebas
probarDispositivoFactory();


//Perifericos de salida
// Clase base para periféricos de salida
abstract class PerifericoSalida {
    abstract tipo: string;
    abstract mostrarInfo(): void;
}
// Clase Monitor
class Monitor extends PerifericoSalida {
    tipo = "Monitor";
    resolucion: string;

    constructor(resolucion: string) {
        super();
        this.resolucion = resolucion;
    }

    mostrarInfo(): void {
        console.log(`Tipo: ${this.tipo}, Resolución: ${this.resolucion}`);
    }
}

// Clase Impresora
class Impresora extends PerifericoSalida {
    tipo = "Impresora";
    tipoImpresion: string;

    constructor(tipoImpresion: string) {
        super();
        this.tipoImpresion = tipoImpresion;
    }

    mostrarInfo(): void {
        console.log(`Tipo: ${this.tipo}, Tipo de Impresión: ${this.tipoImpresion}`);
    }
}

// Clase Proyector
class Proyector extends PerifericoSalida {
    tipo = "Proyector";
    brillo: number;

    constructor(brillo: number) {
        super();
        this.brillo = brillo;
    }

    mostrarInfo(): void {
        console.log(`Tipo: ${this.tipo}, Brillo: ${this.brillo} lúmenes`);
    }
}
class PerifericoSalidaFactory {
    crearPeriferico(tipo: string, opciones: any): PerifericoSalida | null {
        switch (tipo) {
            case "Monitor":
                return new Monitor(opciones.resolucion);
            case "Impresora":
                return new Impresora(opciones.tipoImpresion);
            case "Proyector":
                return new Proyector(opciones.brillo);
            default:
                console.log("Tipo de periférico no soportado");
                return null;
        }
    }
}
// Crear una instancia de la fábrica
const fabrica = new PerifericoSalidaFactory();

// Crear un monitor
const monitor = fabrica.crearPeriferico("Monitor", { resolucion: "1920x1080" });
if (monitor) monitor.mostrarInfo();  // Output: Tipo: Monitor, Resolución: 1920x1080

// Crear una impresora
const impresora = fabrica.crearPeriferico("Impresora", { tipoImpresion: "Láser" });
if (impresora) impresora.mostrarInfo();  // Output: Tipo: Impresora, Tipo de Impresión: Láser

// Crear un proyector
const proyector = fabrica.crearPeriferico("Proyector", { brillo: 3000 });
if (proyector) proyector.mostrarInfo();  // Output: Tipo: Proyector, Brillo: 3000 lúmenes
interface Observador {
    actualizar(equipo: Equipo): void;
}



//Observer
//Ejercicio 1

class DepartamentoMantenimiento implements Observador {
    actualizar(equipo: Equipo): void {
        console.log(`Notificación: El equipo ${equipo.nombre} necesita mantenimiento preventivo.`);
    }
}
class Equipo {
    nombre: string;
    tipo: string;
    estado: string;
    tiempoUso: number;
    umbralMantenimiento: number;
    observadores: Observador[] = [];

    constructor(nombre: string, tipo: string, estado: string, tiempoUso: number, umbralMantenimiento: number) {
        this.nombre = nombre;
        this.tipo = tipo;
        this.estado = estado;
        this.tiempoUso = tiempoUso;
        this.umbralMantenimiento = umbralMantenimiento;
    }

    agregarObservador(observador: Observador): void {
        this.observadores.push(observador);
    }

    notificarObservadores(): void {
        this.observadores.forEach((observador) => observador.actualizar(this));
    }

    aumentarTiempoUso(horas: number): void {
        this.tiempoUso += horas;
        if (this.tiempoUso >= this.umbralMantenimiento) {
            this.notificarObservadores();
        }
    }
}
// Crear el equipo
const equipo1 = new Equipo("Torno", "Mecánico", "Operativo", 95, 100);

// Crear el observador (departamento de mantenimiento)
const departamentoMantenimiento = new DepartamentoMantenimiento();

// Agregar el observador al equipo
equipo1.agregarObservador(departamentoMantenimiento);

// Aumentar el tiempo de uso para disparar la notificación
equipo1.aumentarTiempoUso(10);  // Output: Notificación: El equipo Torno necesita mantenimiento preventivo.

//Ejercicio 2

interface Observador {
    actualizar(inventario: Inventario): void;
}

class InterfazUsuario implements Observador {
    nombre: string;

    constructor(nombre: string) {
        this.nombre = nombre;
    }

    actualizar(inventario: Inventario): void {
        console.log(`Interfaz ${this.nombre} actualizada. Inventario actual:`);
        inventario.listarEquipos();
    }
}
class Inventario {
    private equipos: string[] = [];
    private observadores: Observador[] = [];

    agregarObservador(observador: Observador): void {
        this.observadores.push(observador);
    }

    eliminarObservador(observador: Observador): void {
        const index = this.observadores.indexOf(observador);
        if (index !== -1) {
            this.observadores.splice(index, 1);
        }
    }

    notificarObservadores(): void {
        this.observadores.forEach(observador => observador.actualizar(this));
    }

    agregarEquipo(equipo: string): void {
        this.equipos.push(equipo);
        this.notificarObservadores();
    }

    eliminarEquipo(equipo: string): void {
        const index = this.equipos.indexOf(equipo);
        if (index !== -1) {
            this.equipos.splice(index, 1);
            this.notificarObservadores();
        }
    }

    listarEquipos(): void {
        console.log(this.equipos.join(", "));
    }
}
class Inventario {
    private equipos: string[] = [];
    private observadores: Observador[] = [];

    agregarObservador(observador: Observador): void {
        this.observadores.push(observador);
    }

    eliminarObservador(observador: Observador): void {
        const index = this.observadores.indexOf(observador);
        if (index !== -1) {
            this.observadores.splice(index, 1);
        }
    }

    notificarObservadores(): void {
        this.observadores.forEach(observador => observador.actualizar(this));
    }

    agregarEquipo(equipo: string): void {
        this.equipos.push(equipo);
        this.notificarObservadores();
    }

    eliminarEquipo(equipo: string): void {
        const index = this.equipos.indexOf(equipo);
        if (index !== -1) {
            this.equipos.splice(index, 1);
            this.notificarObservadores();
        }
    }

    listarEquipos(): void {
        console.log(this.equipos.join(", "));
    }
}
interface IFacturacion {
    generarFactura(idProducto: number, cantidad: number): void;
    consultarFactura(idFactura: number): string;
}
class FacturacionVieja {
    crearFactura(idProducto: number, cantidad: number): void {
        console.log(`Factura creada en el sistema antiguo para el producto ${idProducto} con cantidad ${cantidad}.`);
    }

    obtenerFactura(idFactura: number): string {
        return `Factura ${idFactura} obtenida del sistema antiguo.`;
    }
}
class AdaptadorFacturacion implements IFacturacion {
    private facturacionVieja: FacturacionVieja;

    constructor(facturacionVieja: FacturacionVieja) {
        this.facturacionVieja = facturacionVieja;
    }

    generarFactura(idProducto: number, cantidad: number): void {
        this.facturacionVieja.crearFactura(idProducto, cantidad);
    }

    consultarFactura(idFactura: number): string {
        return this.facturacionVieja.obtenerFactura(idFactura);
    }
}
//Ejercicio 2
interface IProveedor {
    obtenerProductos(): any[];
    actualizarInventario(idProducto: number, cantidad: number): void;
}
class ProveedorExternoAPI {
    fetchProductos(): any[] {
        // Simulando una respuesta desde la API externa
        return [
            { productId: 1, name: "Producto A", stock: 50 },
            { productId: 2, name: "Producto B", stock: 30 },
        ];
    }

    updateStock(productId: number, cantidad: number): void {
        console.log(`Stock actualizado en la API externa para el producto ${productId} con cantidad ${cantidad}.`);
    }
}
class AdaptadorProveedor implements IProveedor {
    private proveedorExterno: ProveedorExternoAPI;

    constructor(proveedorExterno: ProveedorExternoAPI) {
        this.proveedorExterno = proveedorExterno;
    }

    obtenerProductos(): any[] {
        // Adaptar los datos recibidos de la API externa al formato requerido por el sistema de inventario
        const productosExternos = this.proveedorExterno.fetchProductos();
        return productosExternos.map(producto => ({
            id: producto.productId,
            nombre: producto.name,
            stock: producto.stock,
        }));
    }

    actualizarInventario(idProducto: number, cantidad: number): void {
        // Adaptar la solicitud de actualización de stock al formato de la API externa
        this.proveedorExterno.updateStock(idProducto, cantidad);
    }
}

```
