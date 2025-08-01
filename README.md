import java.io.*;
import java.net.*;
import java.util.*;

public class Cliente {
    public static void main(String[] args) throws IOException {

        Socket socket = new Socket("localhost", 5000);

        DataInputStream in = new DataInputStream(socket.getInputStream());

        DataOutputStream out = new DataOutputStream(socket.getOutputStream());

        Scanner sc = new Scanner(System.in);

        boolean activo = true;

        while (activo) {
 
           String menu = in.readUTF();
           System.out.println(menu);

            System.out.print("OPCIÓN: ");
            int opcion = sc.nextInt();
            out.writeInt(opcion);

            switch (opcion) {
                case 1: // Suma
                    System.out.print("¿CUANTAS CIFRAS A SUMAR? ");
                    int cantidad = sc.nextInt();
                    out.writeInt(cantidad);

                    for (int i = 0; i < cantidad; i++) {
                        System.out.print("NUMERO " + (i + 1) + ": ");
                        double num = sc.nextDouble();
                        out.writeDouble(num);
                    }
                    break;

                case 2: // Seno
                    System.out.print("UN NUMERO: ");
                    double anguloSeno = sc.nextDouble();
                    out.writeDouble(anguloSeno);
                    break;

                case 3: // Coseno
                    System.out.print("OTRO  NUMERO: ");
                    double anguloCos = sc.nextDouble();
                    out.writeDouble(anguloCos);
                    break;

                case 4: // RAIZ CUADRADA
                    System.out.print("NUMERO POSITIVO: ");
                    double numRaiz = sc.nextDouble();
                    out.writeDouble(numRaiz);
                    break;

                case 5: // POTENCIA
                    System.out.print("BASE: ");
                    double base = sc.nextDouble();
                    System.out.print("EXPONENTE: ");
                    double exponente = sc.nextDouble();
                    out.writeDouble(base);
                    out.writeDouble(exponente);
                    break;

                case 6: // SALIR
                    activo = false;
                    break;

                default:
                    System.out.println("NO VALIDO");
            }

            if (opcion >= 1 && opcion <= 5) {
                String respuesta = in.readUTF();
                System.out.println("Resultado: " + respuesta);
            }
        }

        socket.close();
        sc.close();
    }
}


import java.io.*;
import java.net.*;
import java.util.*;
import java.lang.Math;

public class Servidor {
    public static void main(String[] args) throws IOException {

        ServerSocket servidor = new ServerSocket(5000);
        System.out.println("Esperando conexión...........");

        Socket socket = servidor.accept();
        System.out.println("CONECTADO");

        DataInputStream in = new DataInputStream(socket.getInputStream());
        DataOutputStream out = new DataOutputStream(socket.getOutputStream());

        boolean activo = true;

        while (activo) {

            String menu = "\nMENU: \n1SUMA:\n2SENO:\n3COSENO:\n4RAIZ CUADRADA:\n5POTENCIA:\n6SALIR:";
            out.writeUTF(menu);
            System.out.println(" MENU LLEGO A CLIENTE...");

            System.out.println("CLIENTE TOMANDO OPCIÓN...");
            int opcion = in.readInt();
            System.out.println("DECIDIO: " + opcion);

            switch (opcion) {

                case 1: // Suma
                    System.out.println("OPCIÓN: SUMA ");
                    int cantidad = in.readInt();
                    ArrayList<Double> numeros = new ArrayList<>();
                    double suma = 0;

                    System.out.println(" CORRIENDO...");
                    for (int i = 0; i < cantidad; i++) {
                        double num = in.readDouble();
                        numeros.add(num);
                        suma += num;
                    }

                    String resultadoSuma = "RESULTADO SM: " + suma;
                    out.writeUTF(resultadoSuma);
                    System.out.println("ENVIANDO...");
                    break;

                case 2: // Seno
                    System.out.println("SELECCIÓN SENO");
                    System.out.println("VALIDANDO...");
                    double anguloSeno = in.readDouble();
                    String resultadoSeno = "EL SENO ES " + anguloSeno + " es: " + Math.sin(anguloSeno);
                    out.writeUTF(resultadoSeno);
                    System.out.println("ENVIANDO...");
                    break;

                case 3: // Coseno
                    System.out.println(" OPCIÓN: CCOSENO");
                    System.out.println(" VALIDANDO...");
                    double anguloCos = in.readDouble();
                    String resultadoCos = "COSENO ES " + anguloCos + " es: " + Math.cos(anguloCos);
                    out.writeUTF(resultadoCos);
                    System.out.println("ENVIANO...");
                    break;

                case 4: // Raíz cuadrada
                    System.out.println(" OPCIÓN: RAIZ CUADRADA");
                    System.out.println(" VALIDANDO...");
                    double numRaiz = in.readDouble();
                    if (numRaiz >= 0) {
                        out.writeUTF("LA RAIZ ES " + numRaiz + " es: " + Math.sqrt(numRaiz));
                    } else {
                        out.writeUTF("NUMERO POSITIVO.");
                    }
                    System.out.println(" ENVIANDO...");
                    break;

                case 5: // Potencia
                    System.out.println("OPCION: POTENCIA");
                    System.out.println(" VALIDANDO...");
                    double base = in.readDouble();
                    double exponente = in.readDouble();
                    String resultadoPot = base + " ELEVADO " + exponente + " es: " + Math.pow(base, exponente);
                    out.writeUTF(resultadoPot);
                    System.out.println(" ENVIANDO...");
                    break;

                case 6: // Salir
                    System.out.println(" SE SALIO.");
                    out.writeUTF("SIN CONEXIÓN.");
                    activo = false;
                    break;

                default:
                    out.writeUTF("NO VALIDO.");
                    System.out.println("NO VALIDO.");
            }
        }

        socket.close();
        servidor.close();
        System.out.println(" CONEXIÓN TERMINO.");
    }
}

