# Restaurant-Order-SYstem
import java.sql.*;

public class RestaurantOrderSystem {
    static final String JDBC_URL = "jdbc:postgresql://localhost:5432/ROS";
    static final String USERNAME = "Aisana";
    static final String PASSWORD = "qwerty123";

    public static void main(String[] args) {
        try (Connection connection = DriverManager.getConnection(JDBC_URL, USERNAME, PASSWORD)) {
            Meal meal = new Meal("Pizza", 10.99);
            createMeal(connection, meal);
            ResultSet resultSet = getAllMeals(connection);
            while (resultSet.next()) {
                // Process each meal
                String name = resultSet.getString("name");
                double price = resultSet.getDouble("price");
                System.out.println("Meal: " + name + ", Price: $" + price);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    static void createMeal(Connection connection, Meal meal) throws SQLException {
        String sql = "INSERT INTO meals (name, price) VALUES (?, ?)";
        try (PreparedStatement statement = connection.prepareStatement(sql)) {
            statement.setString(1, meal.getName());
            statement.setDouble(2, meal.getPrice());
            statement.executeUpdate();
        }
    }
    static ResultSet getAllMeals(Connection connection) throws SQLException {
        String sql = "SELECT * FROM meals";
        Statement statement = connection.createStatement();
        return statement.executeQuery(sql);
    }
}
class Meal {
    private String name;
    private double price;
    public Meal(String name, double price) {
        this.name = name;
        this.price = price;
    }
    public String getName() {
        return name;
    }
    public double getPrice() {
        return price;
    }
    static void updateMeal(Connection connection, int mealId, Meal newMealDetails) throws SQLException {
        String sql = "UPDATE meals SET name = ?, price = ? WHERE id = ?";
        try (PreparedStatement statement = connection.prepareStatement(sql)) {
            statement.setString(1, newMealDetails.getName());
            statement.setDouble(2, newMealDetails.getPrice());
            statement.setInt(3, mealId);
            statement.executeUpdate();
        }
    }

    // Method to delete a meal
    static void deleteMeal(Connection connection, int mealId) throws SQLException {
        String sql = "DELETE FROM meals WHERE id = ?";
        try (PreparedStatement statement = connection.prepareStatement(sql)) {
            statement.setInt(1, mealId);
            statement.executeUpdate();
        }
    }
}
