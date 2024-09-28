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

```
