- 👋 Hi, I’m @tracdon
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
tracdon/tracdon is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import random
import time

SEQUENCE_CIBLE = [55, 60, 83, 32, 17]
TAILLE_POPULATION = 500  # Augmenter la taille de la population
GENERATIONS_MAX = 5000  # Augmenter le nombre de générations
TAUX_MUTATION = 0.3  # Augmenter le taux de mutation

def generer_sequence(seed):
    random.seed(seed)
    return sorted(random.sample(range(1, 91), 5))

def fitness(sequence):
    return -sum(abs(a - b) for a, b in zip(sorted(sequence), sorted(SEQUENCE_CIBLE)))

def croisement(parent1, parent2):
    point = random.randint(0, len(parent1) - 1)
    return parent1[:point] + parent2[point:]

def mutation(individu):
    if random.random() < TAUX_MUTATION:
        index = random.randint(0, len(individu) - 1)
        individu[index] = random.randint(1, 1000000)
    return individu

def algorithme_genetique():
    population = [[random.randint(1, 1000000)] for _ in range(TAILLE_POPULATION)]
    
    for generation in range(GENERATIONS_MAX):
        population_evaluee = [(individu, fitness(generer_sequence(individu[0]))) for individu in population]
        population_triee = sorted(population_evaluee, key=lambda x: x[1], reverse=True)
        
        meilleur_individu, meilleur_fitness = population_triee[0]
        meilleure_sequence = generer_sequence(meilleur_individu[0])
        
        if meilleure_sequence == SEQUENCE_CIBLE:
            return meilleur_individu[0], meilleure_sequence, generation
        
        if generation % 100 == 0:
            print(f"Génération {generation}: Meilleure graine = {meilleur_individu[0]}, Séquence = {meilleure_sequence}")
        
        # Créer une nouvelle population avec croisement et mutation
        nouvelle_population = [meilleur_individu]
        while len(nouvelle_population) < TAILLE_POPULATION:
            parent1 = random.choice(population_triee[:20])[0]
            parent2 = random.choice(population_triee[:20])[0]
            enfant = croisement(parent1, parent2)
            enfant = mutation(enfant)
            nouvelle_population.append(enfant)
        
        population = nouvelle_population
    
    return None, None, GENERATIONS_MAX

# Lancer l'algorithme
print("Recherche en cours avec l'algorithme génétique amélioré...")
start_time = time.time()
graine_trouvee, sequence_trouvee, generations = algorithme_genetique()
temps_ecoule = time.time() - start_time

if graine_trouvee:
    print(f"Graine trouvée : {graine_trouvee}")
    print(f"Séquence trouvée : {sequence_trouvee}")
    print(f"Nombre de générations : {generations}")
else:
    print("Aucune graine exacte trouvée")
    print("Meilleure approximation trouvée dans la dernière génération")

print(f"Temps écoulé : {temps_ecoule:.2f} secondes")

