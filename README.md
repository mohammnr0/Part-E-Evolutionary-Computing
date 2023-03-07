import random

# define the target string and other parameters
target_string = "VIJANDER SINGH 200135"
population_size = 1000
mutation_rate = 0.01
max_generations = 1000

# generate a random string
def generate_random_string(length):
    return "".join(random.choices("ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789 ", k=length))

# initialize the population
def initialize_population(size, length):
    return [generate_random_string(length) for i in range(size)]

# calculate the fitness of an individual
def calculate_fitness(individual, target):
    score = sum([1 for i in range(len(target)) if individual[i] == target[i]])
    return score / len(target)

# select a parent using tournament selection
def tournament_selection(population, target, tournament_size):
    tournament = random.sample(population, tournament_size)
    return max(tournament, key=lambda individual: calculate_fitness(individual, target))


def single_point_crossover(parent1, parent2):
    crossover_point = random.randint(1, len(parent1) - 1)
    child1 = parent1[:crossover_point] + parent2[crossover_point:]
    child2 = parent2[:crossover_point] + parent1[crossover_point:]
    return (child1, child2)


def random_mutation(individual, mutation_rate):
    mutated = ""
    for char in individual:
        if random.random() < mutation_rate:
            mutated += random.choice("ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789 ")
        else:
            mutated += char
    return mutated


def run_genetic_algorithm(target, population_size, mutation_rate, max_generations):
   
    population = initialize_population(population_size, len(target))

   
    for generation in range(max_generations):
       
        fitness_scores = [calculate_fitness(individual, target) for individual in population]
        best_fitness = max(fitness_scores)
        best_individual = population[fitness_scores.index(best_fitness)]

       
        if best_individual == target:
            return best_individual

        
        new_population = []
        for i in range(population_size):
            parent1 = tournament_selection(population, target, 5)
            parent2 = tournament_selection(population, target, 5)
            offspring1, offspring2 = single_point_crossover(parent1, parent2)
            offspring1 = random_mutation(offspring1, mutation_rate)
            offspring2 = random_mutation(offspring2, mutation_rate)
            new_population += [offspring1, offspring2]

        
        population = new_population

   
    return best_individual


result = run_genetic_algorithm(target_string, population_size, mutation_rate, max_generations)
print("Best fit string:", result)
