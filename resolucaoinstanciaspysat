import random
import matplotlib.pyplot as plt
from pysat.solvers import Solver
import numpy as np
import time

def generate_sat_instance(n, m, k):
    clauses = set()
    while len(clauses) < m:
        clause = set()
        while len(clause) < k:
            literal = random.randint(1, n)
            if random.random() < 0.5:
                literal = -literal
            if -literal not in clause:
                clause.add(literal)
        clauses.add(tuple(clause))
    return list(clauses)

def is_satisfiable(clauses):
    solver = Solver(name='glucose3')
    for clause in clauses:
        solver.add_clause(clause)
    result = solver.solve()
    solver.delete()
    return result

k = int(input("Digite o número de literais por cláusula (ex: 3 para 3-SAT, 5 para 5-SAT): "))
num_instances = int(input("Digite o número de instâncias a serem testadas: "))
n_values = [50, 100, 150, 200]
alpha_values = np.linspace(3.5, 4.7, 10)

probability_data = {}
time_data = {}

plt.figure(figsize=(8, 6))
for n in n_values:
    probabilities = []
    times = []
    for alpha in alpha_values:
        m = int(alpha * n)
        satisfiable_count = 0
        start_time = time.time()
        for _ in range(num_instances):
            if is_satisfiable(generate_sat_instance(n, m, k)):
                satisfiable_count += 1
        probability = satisfiable_count / num_instances
        elapsed_time = (time.time() - start_time) / num_instances
        probabilities.append(probability)
        times.append(elapsed_time)

    probability_data[n] = probabilities
    time_data[n] = times
    plt.plot(alpha_values, probabilities, marker='o', label=f'n={n}')

critical_alpha = alpha_values[np.argmax(np.diff(probabilities) < -0.5)]
plt.axvline(x=critical_alpha, linestyle='--', color='r', alpha=0.7, label=f'αc ≈ {critical_alpha:.2f}')

plt.xlabel('Razão α = m/n')
plt.ylabel('Probabilidade de SAT')
plt.title(f'Transição de Fase no Problema de {k}-SAT')
plt.legend()
plt.grid(True)
plt.show()

plt.figure(figsize=(8, 6))
for n in n_values:
    plt.plot(alpha_values, time_data[n], marker='s', label=f'n={n}')

plt.xlabel('Razão α = m/n')
plt.ylabel('Tempo médio de execução (s)')
plt.title(f'Tempo Médio de Execução no Problema de {k}-SAT')
plt.legend()
plt.grid(True)
plt.show()
