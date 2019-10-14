class Node():
    def __init__(self, value, cost, pos):
        self.value = value
        self.next = None
        self.cost = cost
        self.pos = pos
    def get_value(self):
    	return self.value
    def get_cost(self):
    	return self.cost
    def get_pos(self):
    	return self.pos
    def get_next(self):
        return self.next
    def set_next(self, new_next):
        self.next = new_next




class LinkedList():
	def __init__(self):
		self.head = None

	def insertAt(self, prevNode, currentNode, cost, value, pos):
		if currentNode.get_value() > value:
			if currentNode.get_next() == None:
				currentNode.set_next(Node(value, cost, pos))
			else:
				self.insertAt(currentNode, currentNode.get_next(), cost, value, pos)
		else:
			newCurrent = Node(value, cost, pos)
			prevNode.set_next(newCurrent)
			newCurrent.set_next(currentNode)


	def insert(self, cost, value, pos):
		if self.head == None: #list is empty
			self.head = Node(value, cost, pos)
		else:
			if self.head.get_value() > value:
				self.insertAt(None, self.head, cost, value, pos)
			else:
				newHead = Node(value, cost, pos)
				newHead.set_next(self.head)
				self.head = newHead

	def print(self):
		currentNode = self.head
		while (currentNode != None):
			print(currentNode.get_cost(), currentNode.get_value())
			currentNode = currentNode.get_next()

	def get_head(self):
		return self.head
