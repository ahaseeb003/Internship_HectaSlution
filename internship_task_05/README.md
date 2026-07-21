# Regression Modeling: From Scratch to Production

## Internship Task 05: Medical Insurance Cost Prediction

### Project Overview
This project is a core component of the 3-Month Machine Learning Internship Program, designed to elevate intermediate Python/ML learners into Junior Machine Learning Engineers. The primary objective is to implement linear and regularized regression models from scratch, benchmark their performance against scikit-learn implementations, and conduct thorough diagnostics and residual analysis. The ultimate goal is to predict medical insurance charges based on various personal attributes, culminating in a production-grade Streamlit application.

### Learning Objectives
By completing this task, the following learning objectives are achieved:
- **Translate Business Problems**: Convert a vague business problem (premium optimization for a health insurer) into a concrete ML/data problem statement.
- **Industry-Standard Tooling & Practices**: Apply best practices in regression modeling, data preprocessing, and model evaluation.
- **Defend Technical Decisions**: Justify technical choices with data, charts, and statistical evidence.
- **Clean, Modular Code**: Produce well-documented, maintainable code suitable for review by senior engineers.
- **Non-Technical Communication**: Effectively communicate findings and model insights to non-technical stakeholders through reports and visuals.
- **Structured Submission**: Push a clean, well-structured submission to a GitHub repository following standard naming conventions.

### Dataset
The project utilizes the **Medical Cost Personal Dataset** (`insurance.csv`), publicly available from Kaggle [1]. This dataset contains 1,338 records of US health insurance beneficiaries with the following attributes:
- `age`: age of primary beneficiary
- `sex`: insurance contractor gender, female, male
- `bmi`: Body mass index, providing an understanding of body, weights that are relatively high or low relative to height, objective index of body weight (kg / m^2) using the ratio of height to weight, ideally 18.5 to 24.9
- `children`: number of children covered by health insurance / number of dependents
- `smoker`: smoking status
- `region`: residential area of the beneficiary in the US, northeast, southeast, southwest, northwest.
- `charges`: individual medical costs billed by health insurance (target variable)

### Project Structure
```
internship_task_05/
├── data/
│   └── insurance.csv
├── notebooks/
│   ├── linear_regression_scratch.py
│   └── regression_from_scratch_to_production.ipynb
├── app/
│   └── app.py
├── docs/
│   └── project_documentation.md
└── README.md
```

### How to Run the Google Colab Notebook
1.  **Open the Notebook**: Navigate to `notebooks/regression_from_scratch_to_production.ipynb` in a Google Colab environment.
2.  **Upload Data**: Ensure `insurance.csv` is uploaded to the Colab environment or mounted from Google Drive in the appropriate path (`/content/insurance.csv` if running directly, or adjust path if using a different setup).
3.  **Run Cells**: Execute all cells sequentially. The notebook will perform data loading, preprocessing, implement regression models from scratch, benchmark against scikit-learn, and conduct diagnostic analysis.

### How to Run the Streamlit Application
1.  **Install Dependencies**: Ensure you have Streamlit, pandas, numpy, scikit-learn, matplotlib, and seaborn installed. You can install them using pip:
    ```bash
    pip install streamlit pandas numpy scikit-learn matplotlib seaborn
    ```
2.  **Navigate to App Directory**: Open your terminal or command prompt and change the directory to `internship_task_05/app`.
    ```bash
    cd internship_task_05/app
    ```
3.  **Run the App**: Execute the Streamlit application:
    ```bash
    streamlit run app.py
    ```
4.  **Access the App**: The application will open in your web browser, typically at `http://localhost:8501`.

### Streamlit App Features
The Streamlit application provides two main functionalities:
1.  **Interactive Visualization**: Explore the dataset through dynamic charts and sliders. Understand the distribution of insurance charges and how key features like age, BMI, and smoking status correlate with costs.
2.  **Real-world Prediction**: Utilize the trained linear regression model to predict medical insurance charges based on user-inputted parameters. The prediction includes a comparison against the average insurance cost.

### Technologies Used
-   **Python**
-   **NumPy**: For numerical operations in from-scratch implementations.
-   **Pandas**: For data manipulation and analysis.
-   **Scikit-learn**: For benchmarking and standard ML utilities.
-   **Matplotlib & Seaborn**: For data visualization.
-   **Streamlit**: For building interactive web applications.
-   **nbformat**: For programmatically generating the Colab notebook.

### Benchmarking and Diagnostics
The project includes a thorough comparison between custom-built linear, Ridge, and Lasso regression models and their scikit-learn counterparts. Performance is evaluated using metrics such as Mean Squared Error (MSE), Root Mean Squared Error (RMSE), Mean Absolute Error (MAE), and R-squared (R2). Additionally, residual plots and Q-Q plots are generated to assess model assumptions and identify potential issues, while feature importance is analyzed through model coefficients.

### Conclusion
This project serves as a comprehensive demonstration of regression modeling principles, from foundational mathematical implementations to practical deployment in a web application. It highlights the importance of understanding model mechanics, rigorous evaluation, and effective communication of results.

### Author
Abdul Haseeb

### References
[1] Medical Cost Personal Dataset. Kaggle. Available at: [https://www.kaggle.com/datasets/mirichoi0218/insurance](https://www.kaggle.com/datasets/mirichoi0218/insurance)
