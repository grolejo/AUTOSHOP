import java.util.ArrayList;
import java.util.List;

class Vehiculo {
    private String marca;
    private String modelo;
    private int año;
    private double kilometraje;
    private double precio;
    private boolean vendido;

    public Vehiculo(String marca, String modelo, int año, double kilometraje, double precio) {
        this.marca = marca;
        this.modelo = modelo;
        this.año = año;
        this.kilometraje = kilometraje;
        this.precio = precio;
        this.vendido = false;
    }

    public double calcularPrecioVenta() {
        return this.precio * 1.15;
    }

    public void marcarComoVendido() {
        this.vendido = true;
    }

    // Getters y setters para todos los atributos

    @Override
    public String toString() {
        return "Vehiculo{" +
                "marca='" + marca + '\'' +
                ", modelo='" + modelo + '\'' +
                ", año=" + año +
                ", kilometraje=" + kilometraje +
                ", precio=" + precio +
                ", vendido=" + vendido +
                '}';
    }
}

class Cliente {
    private String nombre;
    private String telefono;
    private String email;

    public Cliente(String nombre, String telefono, String email) {
        this.nombre = nombre;
        this.telefono = telefono;
        this.email = email;
    }

    // Getters y setters para todos los atributos

    @Override
    public String toString() {
        return "Cliente{" +
                "nombre='" + nombre + '\'' +
                ", telefono='" + telefono + '\'' +
                ", email='" + email + '\'' +
                '}';
    }
}

class Vendedor {
    private String nombre;
    private List<Venta> ventasRealizadas;

    public Vendedor(String nombre) {
        this.nombre = nombre;
        this.ventasRealizadas = new ArrayList<>();
    }

    public void agregarVenta(Venta venta) {
        this.ventasRealizadas.add(venta);
    }

    // Getters y setters para todos los atributos

    @Override
    public String toString() {
        return "Vendedor{" +
                "nombre='" + nombre + '\'' +
                ", ventasRealizadas=" + ventasRealizadas +
                '}';
    }
}

class Venta {
    private Vehiculo vehiculo;
    private Cliente cliente;
    private Vendedor vendedor;
    private double precioVenta;

    public Venta(Vehiculo vehiculo, Cliente cliente, Vendedor vendedor, double precioVenta) {
        this.vehiculo = vehiculo;
        this.cliente = cliente;
        this.vendedor = vendedor;
        this.precioVenta = precioVenta;
    }

    // Getters y setters para todos los atributos

    @Override
    public String toString() {
        return "Venta{" +
                "vehiculo=" + vehiculo +
                ", cliente=" + cliente +
                ", vendedor=" + vendedor.getNombre() +  // Usamos getNombre() para obtener solo el nombre
                ", precioVenta=" + precioVenta +
                '}';
    }
}

class AutoShop {
    private List<Vehiculo> inventario;
    private List<Cliente> clientes;
    private List<Vendedor> vendedores;

    public AutoShop() {
        this.inventario = new ArrayList<>();
        this.clientes = new ArrayList<>();
        this.vendedores = new ArrayList<>();
    }

    public void agregarVehiculo(Vehiculo vehiculo) {
        this.inventario.add(vehiculo);
    }

    public List<Vehiculo> buscarVehiculo(String marca, String modelo) {
        List<Vehiculo> resultados = new ArrayList<>();
        for (Vehiculo vehiculo : this.inventario) {
            if (vehiculo.getMarca().equalsIgnoreCase(marca) && vehiculo.getModelo().equalsIgnoreCase(modelo)) {
                resultados.add(vehiculo);
            }
        }
        return resultados;
    }

    public Venta registrarVenta(Vehiculo vehiculo, Cliente cliente, Vendedor vendedor) {
        if (this.inventario.contains(vehiculo) && !vehiculo.isVendido()) {
            double precioVenta = vehiculo.calcularPrecioVenta();
            Venta venta = new Venta(vehiculo, cliente, vendedor, precioVenta);
            vendedor.agregarVenta(venta);
            vehiculo.marcarComoVendido();
            return venta;
        } else {
            return null;
        }
    }

    public void generarReporteVentas() {
        System.out.println("Reporte de Ventas:");
        for (Vendedor vendedor : this.vendedores) {
            for (Venta venta : vendedor.getVentasRealizadas()) {
                System.out.println(venta);
            }
        }
    }

    public static void main(String[] args) {
        AutoShop autoShop = new AutoShop();

        // Agregar vehículos
        autoShop.agregarVehiculo(new Vehiculo("Toyota", "Corolla", 2022, 10000, 20000));
        autoShop.agregarVehiculo(new Vehiculo("Honda", "Civic", 2023, 5000, 25000));
        autoShop.agregarVehiculo(new Vehiculo("Ford", "Focus", 2021, 15000, 18000));

        // Crear clientes
        Cliente cliente1 = new Cliente("Juan Perez", "1234567890", "juan.perez@email.com");
        Cliente cliente2 = new Cliente("Maria Garcia", "9876543210", "maria.garcia@email.com");

        // Crear vendedores
        Vendedor vendedor1 = new Vendedor("Pedro Lopez");
        Vendedor vendedor2 = new Vendedor("Ana Gomez");
        autoShop.vendedores.add(vendedor1);
        autoShop.vendedores.add(vendedor2);

        // Registrar ventas
        autoShop.registrarVenta(autoShop.inventario.get(0), cliente1, vendedor1);
        autoShop.registrarVenta(autoShop.inventario.get(1), cliente2, vendedor2);

        // Buscar vehículos
        List<Vehiculo> resultadosBusqueda = autoShop.buscarVehiculo("Toyota", "Corolla");
        for (Vehiculo vehiculo : resultadosBusqueda) {
            System.out.println("Vehículo encontrado: " + vehiculo);
        }

        // Generar reporte de ventas
        autoShop.generarReporteVentas();
    }
}
