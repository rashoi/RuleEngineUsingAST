# Rule Engine with Abstract Syntax Tree (AST)

Welcome to the **Rule Engine with AST** project! This application serves as a robust framework for determining user eligibility based on various criteria such as age, department, income, and spending. By leveraging an Abstract Syntax Tree (AST), the system dynamically manages and evaluates rules, making it a powerful tool for decision-making processes.

## Highlights
- **Dynamic Rule Management**: Create, combine, and evaluate rules seamlessly.
- **User-Friendly Interface**: Built with React.js, providing an engaging experience.
- **Efficient Backend**: Node.js and Express handle API requests and rule processing.
- **Scalable Database**: MongoDB is used for flexible data storage of rules and metadata.

## Technology Stack
- **MongoDB**: A NoSQL database selected for its ease of use and ability to handle complex data structures, making it ideal for storing rules and user data.
- **React.js**: This JavaScript library enables the creation of interactive user interfaces, allowing for responsive and dynamic content.
- **Express.js**: This web application framework simplifies the process of building server-side applications and APIs, facilitating smooth communication between the frontend and backend.
- **Node.js**: As a server-side JavaScript environment, Node.js provides a platform for building scalable applications that handle concurrent requests effectively.
- **Python**: Employed for backend logic, Python is utilized to implement the core functionalities of rule processing and evaluation.
- **JSON**: This lightweight data interchange format is used for data transmission between the client and server, ensuring compatibility and ease of integration.

## Getting Started

### Cloning the Repository
To get started with this project, clone the repository to your local machine:
```bash
git clone https://github.com/rashoi/RuleEngineUsingAST
cd Rule-Engine-AST
```

### Backend Setup (Node.js & Express)
1. Navigate to the backend directory:
   ```bash
   cd backend
   ```
2. Install required dependencies:
   ```bash
   npm install
   ```
3. Start the Express server:
   ```bash
   npm start
   ```

### Frontend Setup (React)
1. Go to the frontend directory:
   ```bash
   cd frontend
   ```
2. Install frontend dependencies:
   ```bash
   npm install
   ```
3. Launch the React application:
   ```bash
   npm start
   ```

### MongoDB Configuration
Ensure MongoDB is installed and running. Create a database for this application and update the connection string in the backend configuration file accordingly.

## How to Use

1. **Creating Rules**: Use the `/create_rule` endpoint to submit rule strings, which will be converted into an AST structure.
   ```python
   import requests

   rule_string = "((age > 30 AND department = 'Sales') OR (age < 25 AND department = 'Marketing')) AND (salary > 50000 OR experience > 5)"
   response = requests.post("http://localhost:5000/create_rule", json={"rule_string": rule_string})
   print(response.json())
   ```

2. **Combining Rules**: Send a list of rule strings to the `/combine_rules` endpoint to create a comprehensive AST representation.
   ```python
   rules = [
       "((age > 30 AND department = 'Sales')) AND (salary > 20000 OR experience > 5)",
       "((age < 25 AND department = 'Marketing'))"
   ]
   response = requests.post("http://localhost:5000/combine_rules", json={"rules": rules})
   print(response.json())
   ```

3. **Evaluating Rules**: Use the `/evaluate_rule` endpoint to check user attributes against the combined rule.
   ```python
   user_data = {"age": 35, "department": "Sales", "salary": 60000, "experience": 3}
   response = requests.post("http://localhost:5000/evaluate_rule", json={"data": user_data})
   print(response.json())
   ```

## API Overview

- **Create Rule**:
   ```python
   @app.route('/create_rule', methods=['POST'])
   def create_rule():
       rule_string = request.json['rule_string']
       ast = build_ast(rule_string)  # Function to construct the AST
       return jsonify(ast.to_dict())
   ```

- **Combine Rules**:
   ```python
   @app.route('/combine_rules', methods=['POST'])
   def combine_rules():
       rules = request.json['rules']
       combined_ast = merge_rules(rules)  # Function to merge ASTs
       return jsonify(combined_ast.to_dict())
   ```

- **Evaluate Rule**:
   ```python
   @app.route('/evaluate_rule', methods=['POST'])
   def evaluate_rule():
       user_data = request.json['data']
       eligibility = assess_rule(ast, user_data)  # Function to evaluate the AST
       return jsonify({"eligible": eligibility})
   ```

## Node Data Structure

The AST is represented using the following `Node` class structure:
```python
class Node:
    def __init__(self, node_type, left=None, right=None, value=None):
        self.type = node_type  # Indicates if it's an operator or operand
        self.left = left  # Pointer to the left child node
        self.right = right  # Pointer to the right child node
        self.value = value  # Holds optional value for operand nodes

    def to_dict(self):
        return {
            "type": self.type,
            "left": self.left.to_dict() if self.left else None,
            "right": self.right.to_dict() if self.right else None,
            "value": self.value
        }
```

## Testing the Application

1. **Individual Rule Creation**: Test the ability to create rules and validate their AST structure.
2. **Rule Combination**: Verify that combined rules accurately represent the intended logic.
3. **Data Evaluation**: Use various JSON datasets to confirm the functionality of the `evaluate_rule` method.

## Potential Improvements
- **Enhanced User Interface**: Develop a more intuitive UI to allow for easier rule creation and management.
- **Complex Rule Handling**: Expand the syntax to support more advanced logical conditions.
- **Integration with External Data**: Enable the engine to evaluate rules against additional data sources for increased versatility.
