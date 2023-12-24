# Sistema-para-estacionamento
Sistema Para Estacionamento DIO 
using System;
using System.Collections.Generic;
using Octokit;

namespace Estacionamento
{
    class Program
    {
        static List<Veiculo> veiculosEstacionados = new List<Veiculo>();

        static void Main(string[] args)
        {
            bool sair = false;
            while (!sair)
            {
                Console.WriteLine("------ Estacionamento ------");
                Console.WriteLine("1 - Adicionar veículo");
                Console.WriteLine("2 - Remover veículo");
                Console.WriteLine("3 - Listar veículos");
                Console.WriteLine("4 - Sair");
                Console.Write("Escolha uma opção: ");

                int opcao;
                bool validOpcao = int.TryParse(Console.ReadLine(), out opcao);

                if (!validOpcao)
                {
                    Console.WriteLine("Opção inválida.");
                    continue;
                }

                switch (opcao)
                {
                    case 1:
                        AdicionarVeiculo();
                        break;
                    case 2:
                        RemoverVeiculo();
                        break;
                    case 3:
                        ListarVeiculos();
                        break;
                    case 4:
                        sair = true;
                        break;
                    default:
                        Console.WriteLine("Opção inválida.");
                        break;
                }

                Console.WriteLine();
            }
        }

        static void AdicionarVeiculo()
        {
            Console.Write("Digite a placa do veículo: ");
            string placa = Console.ReadLine();
            Console.Write("Digite o modelo do veículo: ");
            string modelo = Console.ReadLine();
            veiculosEstacionados.Add(new Veiculo(placa, modelo));
            Console.WriteLine("Veículo adicionado com sucesso.");
        }

        static void RemoverVeiculo()
        {
            Console.Write("Digite a placa do veículo: ");
            string placa = Console.ReadLine();
            Veiculo veiculo = veiculosEstacionados.Find(x => x.Placa == placa);

            if (veiculo == null)
            {
                Console.WriteLine("Veículo não encontrado.");
                return;
            }

            veiculosEstacionados.Remove(veiculo);
            Console.Write("Digite a quantidade de horas estacionado: ");
            int horasEstacionado;
            bool validHorasEstacionado = int.TryParse(Console.ReadLine(), out horasEstacionado);

            if (!validHorasEstacionado)
            {
                Console.WriteLine("Valor inválido.");
                return;
            }

            decimal valorCobrado = CalcularValorCobrado(horasEstacionado);
            Console.WriteLine($"Valor cobrado: R${valorCobrado}");
        }

        static void ListarVeiculos()
        {
            Console.WriteLine("------ Veículos Estacionados ------");

            if (veiculosEstacionados.Count == 0)
            {
                Console.WriteLine("Nenhum veículo estacionado.");
                return;
            }

            foreach (var veiculo in veiculosEstacionados)
            {
                Console.WriteLine($"Placa: {veiculo.Placa} | Modelo: {veiculo.Modelo}");
            }
        }

        static decimal CalcularValorCobrado(int horasEstacionado)
        {
            // Aqui você pode implementar a lógica de cálculo do valor cobrado, de acordo com as regras do estacionamento.
            // Por exemplo, um valor fixo por hora estacionada, ou uma tabela de preços diferenciados por tipo de veículo, etc.
            // Para simplificar o exemplo, vamos assumir um valor fixo de R$5 por hora estacionada.
            return horasEstacionado * 5;
        }
    }

    class Veiculo
    {
        public string Placa { get; set; }
        public string Modelo { get; set; }

        public Veiculo(string placa, string modelo)
        {
            Placa = placa;
            Modelo = modelo;
        }
    }
}
