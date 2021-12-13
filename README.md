- [Maximum-Indepent-Set](#maximum-indepent-set)
- [Fuzzy Genetic Algorithm with rapid Population expansion for Maximum Independent Set Problem](#fuzzy-genetic-algorithm-with-rapid-population-expansion-for-maximum-independent-set-problem)
  * [Components of Algorithm :](#components-of-algorithm--)
    + [Initialization](#initialization--)
    + [Selection](#selection--)
    + [Cross-Over](#cross-over--)
    + [Mutation](#mutation--)
    + [Elitism](#elitism--)
    + [Enhance](#enhance--)
  * [Psuedo-code](#psuedo-code-)
  * [Result](#result)

# Maximum-Independent-Set
A maximal independent set is either an independent set such that adding any other vertex to the set forces the set to contain an edge or the set of all vertices of the empty graph.

A maximum independent set is an independent set of largest possible size for a given graph G. This size is called the independence number of G, and denoted α(G).The problem of finding such a set is called the maximum independent set problem and is an NP-hard optimization problem. As such, it is unlikely that there exists an efficient algorithm for finding a maximum independent set of a graph. (source: wikipedia)

The idea presented below is based on genetic algorithm. The idea is to select a set of independent vertices initally and then improve the selection using steps such as selection of best population (set with highest indepent vertices), cross-over (interchange of vertices between two selected population), mutation, elitism and enhance (attempt to improve population by running bfs). The details of idea is presented below.

# Fuzzy Genetic Algorithm with rapid Population expansion for Maximum Independent Set Problem 

## Components of Algorithm :

The primary idea is based on selection of vertices randomly and the utilizing Genetic Algorithm to improve the selection. The set of no of vectors which conatins information is fixed to 32, it was observed that fixing it at 150 gives better output, however slows down the program. The output obtained by each such iteration is noted and the best output is selected. In each such iteration, the set of binary vectors goes through 64 iterations consistsing of various steps of Genetic Algorithm and Hueristic based on random selection (i.e fuzzy logic) and produces an output. 

### Initialization :
An array of vectors, each containing their set of selected vertices. The selection of vertices for each vector is done by choosing a set of vertices of random size uniformly and is independent of other selections. It is ensured that selected set of vertices always form an independent set. The size of a vector represents no of vertices forming an independent set. 

### Selection :
A tournament sort is conducted among the existing vectors, the vector having more selected vertices are preferred over those having less selected vertices, in case of equal no of selected vertices, both the vectors are reatined. With each iteration, the selection procedure increases the probability selection of optimal binary vector for modification by different stages of Genetic Algorithm.

### Cross-Over :
A uniform cross-over is done with two binary vectors next to each other after selection procedure. All selected vertices from the  vectors are selected and sorted greedily and fuzzily. These sorted vertices thus generates a new vector. Throughout the procedure, it is ensured that no two directly connected vertices are part of set.

### Mutation :
For every vector, with some random probability, the vertices currently set to '0' are sorted randomly and fuzzily. These selected vertices try to populate the vector keeping the invariant in consideration.

### Elitism :
The  vectors from initial population and those after modification are combined and are sorted greedily with prefernce given to those having higher no of set vertices. Half of this population is discarded and the best half goes through another iteration.

In each iteration, if size of set of binary vectors is greater can some fixed value then some intermediate vectors are replaced by new initialized vector. This ensures that a vertex can still get selected after removal of its parent vector in any step before.

### Enhance :
A rapid population expansion is done for a selected vector. This vectors is neither among five best vectors set and nor a part of five worst vector set.

A breadth first search is performed trying to include all possible vertices for a given vector from its currently selected set of vertices. This results in maximum set of selected vertices in any vector having current selected vectors similar to those as given vector. It tries to converge the feasible solution towards the optimal solution. 

A considerable improvement is seen in regular graphs due to this step.

## Psuedo-code:

	Class Graph:
		Variables n, adjacency_matrix, adjacency_list

		function connect(x,y):
			store the connection information in adjacency_matrix and also in adjacency_list

		function is_connected(x,y):
			if adjacency_matrix[x][y] is set to 1:
				return true
			else :
				return false

		function get_degree(x):
			return adjacency_list[x].size

		function resize(no_of_vertices):
			n = no_of_vertices
			Resize other variables so that they can store data for given no of vertices

	//Create a graph object
	Graph graph
	Variable SIZE = 32 // A higher value gives better value at cost of computational time
	Variable vertices

	function input(n):
		Variable No_of_edges,x,y
		Input No_of_edges
		Repeat till No_of_edges > 0:
			Input x,y
			graph.connect(x,y)

	//Main Algorithm
	function initialize(initial_string):
		Variable no_of_selection, vertex, temp_string
		No_of_selection = random()
		Repeat till No_of_selection > 0:
			vertex = random() 
			temp_string[ vertex ] = '1'
			for all connected vertices with vertex, set corresponding vertex in temp_string to '0'
		initial_string = create a vector containing all vertices set


	function initialization( initial_string[] ):
		for i=1 to initial_string.size():
			initialize( initial_string[i] )

	function selection( initial_string[] ):
		for i=1 to initial_string.size():
			if( initial_string[i] < initial_string[i+1] ):
				initial_string[i] = initial_string[i+1]

	function cross-over( initial_string[] ):
		for i=1 to initial_string.size():
			if ( random(1,vertices) < 80 ):
				string_before = initial_string[i]
				temp[] = set of all vertices set in initial_string[i] and initial_string[i+1]
				if(random(1,100) > 80):
					sort temp[] in increasing order of degree
				else:
					sort temp[] in decreasing order of degree
				initial_string[i].clear()
				for x in temp:
					initial_string[i][x]='1'
					for y in adjacency_list[x]:
						initial_string[i][y]='0'
				if (string_before.size() > initial_string[i].size() ):
					initial_string[i] = string_before

	function mutation( initial_string[] ):
		for i=1 to initial_string.size():
			if ( random(1,100) < 20 ):
				temp[] = all vertices not present in initial_string[i]
				if( random(1,100) < 60 ):
					sort temp[] in increasing order of dergee of vertices
				else :
					sort temp[] in decreasing order of dergee of vertices
				for x in temp:
					if x is not connected to any vertice in initial_string[i]:
						add x to initial_string[i]

	function elitism( base_string[], initial_string[] ):
		add all vector from initial_string to base_string
		sort base_string in decreasing order of size of each vector inside it.
		remove all vectors after middle vector.

	function new_population():
		K = random(vertices/2, vertices*0.6)
		temp = {}
		for k times:
			insert( random(1,vertex) ) in temp
		sort temp in increasing order of degree of vertices
		l = random(1, vertices/4)
		l = min(l, temp.size())
		v =[]
		for vertex in temp:
			if( vertex is not connected with any vertex in v):
				add vertex to v
		return v

	function enhance(initial_string):
		exist = {1,2,.....,n} //set of all possible vertices
		temp = {0|1}^n //binary string representing vertices
		remover = initial_string //set of all vertices to be removed
		while (exist is not empty or iterations<500):
			remove all vertices present in remover from exist
			level1 = {}
			for all vertices reachable from any vertex in remover and not present in exist:
				insert this vertex to level1 and remove it from exist
			remover.clear()
			for all vertices reachable from any vertex in level1 and not present in exist:
				mark this vertex as '1' in temp, and unmark all vertices connected to it, erase this vertex from exist, insert this vertex in remover
			if graph is disconnected:
			insert any vertex from exist to remover, mark this vertex in temp
		initial_string.clear()
		for all marked vertex in temp:
			add this to initial_string

	function result(output)
		initial_string = [SIZE]
		initialization( initial_string )
		base_string[] = initial_string[]
		for i=1 to 64:
			selection(initial_strings);
			cross_over(initial_strings);
			mutation(initial_strings);
			elitism(initial_strings, base_string);
			if(SIZE>30):
				new_pop(initial_strings[random(1,SIZE-10)+5]);
				new_pop(initial_strings[random(1,SIZE-10)+5]);
				enhance(initial_strings[random(1,SIZE-10)+5]);
		if (inital_string[0].size() > output.size()):
			output = initial_string[0]
		return initial_string[0].size()

	function main():
		Variable no_of_vertices, iterations_allowed, output[]
		Input no_of_vertices
		iterations_allowed = max(16, no_of_vertices/6)
		iterations_allowed = min(iterations_allowed, 200)
		for i=1 to iterations_allowed:
			result(output)
		print(output.size())
		print (output)
  
## Results
The implemented code was run against (standard) data set avaiable on oeis. The result on regular graphs improved to 100%, however for other random graphs was not good compared to already existing records. The results degraded with increasing density of graphs. 
 
In any case, the result produced by running the algorithm on a set of maximum 32 vertices produced optimal result. And the improved result on regular graph is noteworthy.

The details of input graphs are present in Input folder. It contains randomly generated graphs of 100 and 200 vertices of different densities and regular grpahs of 102, 202...till 1002 vertices. The corresponding output folder contains the details of maximum independent set found and corresponding vertices list. The result folder contains the details (i.e. maximum indepent set found, time taken against each file is present in output folder).
