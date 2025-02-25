import datetime

movimentacoes = []
saques_diarios = 0
limite_saques_dia = 3
limite_valor_saque = 500
saldo = 0
data_ultimo_saque = datetime.date.today()

def mostrar_extrato(movimentacoes):
    if movimentacoes:
        print("Extrato da conta:")
        for transacao in movimentacoes:
            print(transacao)
    else:
        print("Não foram realizadas movimentações.")

def mostrar_saldo(saldo):
    print(f"Seu saldo atual é: R${saldo:.2f}")

def menu_inicial():
    global saldo, saques_diarios, data_ultimo_saque

    print("BANCO MU!")
    
    while True:
        operacao_selecionada = input("Selecione a operação desejada:\n1 para extrato\n2 para depósito\n3 para saque\n4 para sair\n")    

        if operacao_selecionada == '1':
            mostrar_extrato(movimentacoes)
            mostrar_saldo(saldo)
        
        elif operacao_selecionada == '2':
            valor_deposito = input("Valor do depósito: R$")
            try:
                valor_deposito = float(valor_deposito)
                if valor_deposito > 0:
                    saldo += valor_deposito
                    now = datetime.datetime.now()
                    movimentacoes.append(f"+R${valor_deposito:.2f} ({now.strftime('%d-%m-%Y %H:%M')})")
                    print(f"Operação realizada com sucesso. Seu saldo atual é: R${saldo:.2f}")
                else:
                    print("Não é possível inserir um valor negativo, tente novamente.")
            except ValueError:
                print("Por favor, insira um número válido.")
        
        elif operacao_selecionada == '3':
            if saques_diarios < limite_saques_dia:
                valor_saque = input("Valor do saque: R$")
                try:
                    valor_saque = float(valor_saque)
                    if valor_saque > 0:
                        if valor_saque <= saldo:
                            if valor_saque <= limite_valor_saque:
                                saldo -= valor_saque
                                saques_diarios += 1
                                now = datetime.datetime.now()
                                movimentacoes.append(f"-R${valor_saque:.2f} ({now.strftime('%d-%m-%Y %H:%M')})")
                                print(f"Saque de R${valor_saque:.2f} realizado com sucesso. Seu saldo atual é: R${saldo:.2f}")
                            else:
                                print(f"O valor do saque não pode ser superior a R${limite_valor_saque:.2f}.")
                        else:
                            print("Saldo insuficiente para realizar o saque.")
                    else:
                        print("O valor do saque deve ser positivo.")
                except ValueError:
                    print("Por favor, insira um número válido para o saque.")
            else:
                print("Você atingiu o limite de saques diários.")
        
        elif operacao_selecionada == '4':
                    print("******* Eu não sou o BANCO MU, mas fui feito para você. *******")
                    print("******* Saindo do Banco MU! Até logo! *******")
                    break  # Sai do loop e encerra o programa
        
        else:
                   print("Opção inválida. Tente novamente.")

menu_inicial()