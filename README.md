# Detecção de Fraudes em Cartões de Crédito com Machine Learning

*Autor: Eduardo Pires Gomes Cruz Filho*

Contexto: Projeto de Prevenção a Fraudes e Blindagem Financeira

1. Introdução e Entendimento do Negócio
A fraude em transações de cartão de crédito é um dos problemas mais críticos e dispendiosos enfrentados por instituições financeiras e e-commerces. O grande desafio técnico deste projeto reside na natureza do problema: o extremo desbalanceamento dos dados. Em cenários reais, as fraudes representam uma fração ínfima do volume total de transações — neste dataset específico, apenas 0,172% das observações pertencem à classe de fraude (Classe 1).

Se utilizássemos algoritmos tradicionais sem o devido tratamento, um modelo "burro" que classificasse todas as transações como legítimas alcançaria uma acurácia de 99,828%. No entanto, o prejuízo financeiro continuaria idêntico, tornando a acurácia uma métrica proibida para este escopo.

🎯 Objetivo Principal
O foco central deste projeto é Minimizar os Falsos Negativos (elevar o Recall). Em termos de negócio, um Falso Negativo significa uma fraude que passou batida pelo sistema, gerando prejuízo financeiro direto e estorno (chargeback). Contudo, buscamos fazer isso sem destruir a Precisão, garantindo que clientes legítimos não sofram bloqueios indevidos e fricção desnecessária na sua experiência de compra.

2. Resumo da Jornada: Do Dado Bruto ao Modelo de Alta Performance
Para alcançar um modelo robusto, seguro e generalista, cruzamos as seguintes etapas do ciclo de vida de Ciência de Dados:

Preparação de Dados & Escalonamento: As variáveis V1 a V28 (já tratadas previamente via PCA) estavam em uma escala completamente diferente de Time e Amount. Para neutralizar o efeito de grandes outliers (compras de valores bizarramente altos), aplicamos o RobustScaler.

Divisão Estratificada (Split): Para garantir a idoneidade da validação, isolamos 20% dos dados para testes utilizando o StratifiedShuffleSplit, mantendo a proporção exata de 0,172% de fraudes tanto no treino quanto no teste. A base de teste permaneceu intocada para simular o "mundo real".

Seleção de Modelos (A Bancada de Testes): Testamos três abordagens lidando nativamente com o desbalanceamento através de pesos de classe (class_weight e scale_pos_weight): Regressão Logística (baseline), Random Forest e XGBoost. O XGBoost se destacou pelo melhor equilíbrio inicial entre Precisão e Recall.

Otimização de Hiperparâmetros (Tuning): Utilizando o RandomizedSearchCV, tunamos os parâmetros do XGBoost (como profundidade, taxa de aprendizado e subamostragem) focando no F1-Score. Isso gerou um "motor" muito mais inteligente e capaz de separar as probabilidades de forma assertiva.

Calibração do Limiar de Decisão (Threshold): Em vez de aceitar o corte padrão de 50%, analisamos a curva de Trade-off entre Precisão e Recall. Descobrimos que o modelo tunado operava em nível de excelência ao adotar um Threshold cirúrgico de 98,13%.

📊 Resultado da Validação Final (Dados Inéditos)
Ao submeter o modelo final calibrado a um lote de 11.902 transações inéditas, obtivemos resultados de nível sênior:

90,6% de Recall: Detectamos 29 das 32 fraudes reais.

90,6% de Precisão: Geramos apenas 3 alarmes falsos em quase 12 mil compras legítimas (taxa de erro de experiência do usuário de apenas 0,02%).
