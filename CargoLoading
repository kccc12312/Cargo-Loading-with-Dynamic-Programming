
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

class CargoItem {
    String name;
    int weight;
    int priorityValue;
    int index; // Index to keep track of original order

    public CargoItem(String name, int weight, int priorityValue, int index) {
        this.name = name;
        this.weight = weight;
        this.priorityValue = priorityValue;
        this.index = index;
    }

    @Override
    public String toString() {
        return "name=" + name + ", weight=" + weight + ", priorityValue=" + priorityValue;
    }
}

public class CargoLoading {
    public static void main(String[] args) {
        String csvFile = "\cargoitems.csv"; // Path to your CSV file
        int maxWeight = 4000; // Maximum weight capacity of the ship
        List<CargoItem> items = loadItemsFromCsv(csvFile);

        Result result = knapsackDynamicProgramming(items, maxWeight);

        System.out.println("Total Priority Value: " + result.totalPriorityValue);
        System.out.println("Total Weight: " + result.totalWeight);
        System.out.println("Items Chosen:");

        // Sort the itemsChosen list by priority value in descending order
        Collections.sort(result.itemsChosen, new Comparator<CargoItem>() {
            @Override
            public int compare(CargoItem item1, CargoItem item2) {
                return Integer.compare(item2.priorityValue, item1.priorityValue);
            }
        });

        for (int i = 0; i < result.itemsChosen.size(); i++) {
            CargoItem item = result.itemsChosen.get(i);
            System.out.println((i + 1) + ". Name: " + item.name + ", Priority Value: " + item.priorityValue + ", Weight: " + item.weight);
        }
    }

    private static List<CargoItem> loadItemsFromCsv(String csvFile) {
        List<CargoItem> items = new ArrayList<>();
        String line;
        String csvSplitBy = ",";

        try (BufferedReader br = new BufferedReader(new FileReader(csvFile))) {
            br.readLine(); // skip header
            int index = 1;
            while ((line = br.readLine()) != null) {
                String[] data = line.split(csvSplitBy);
                String name = data[0];
                int weight = Integer.parseInt(data[1]);
                int priorityValue = Integer.parseInt(data[2]);
                items.add(new CargoItem(name, weight, priorityValue, index++));
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Sort items by priority value in descending order
        Collections.sort(items, new Comparator<CargoItem>() {
            @Override
            public int compare(CargoItem item1, CargoItem item2) {
                return Integer.compare(item2.priorityValue, item1.priorityValue);
            }
        });

        return items;
    }

    private static Result knapsackDynamicProgramming(List<CargoItem> items, int maxWeight) {
        int n = items.size();
        int[][] dp = new int[n + 1][maxWeight + 1];

        for (int i = 1; i <= n; i++) {
            CargoItem item = items.get(i - 1);
            for (int w = 1; w <= maxWeight; w++) {
                if (item.weight <= w) {
                    dp[i][w] = Math.max(dp[i - 1][w], dp[i - 1][w - item.weight] + item.priorityValue);
                } else {
                    dp[i][w] = dp[i - 1][w];
                }
            }
        }

        int totalPriorityValue = dp[n][maxWeight];
        int totalWeight = 0;
        List<CargoItem> itemsChosen = new ArrayList<>();

        for (int i = n, w = maxWeight; i > 0; i--) {
            if (dp[i][w] != dp[i - 1][w]) {
                CargoItem item = items.get(i - 1);
                itemsChosen.add(item);
                totalWeight += item.weight;
                w -= item.weight;
            }
        }

        return new Result(totalPriorityValue, totalWeight, itemsChosen);
    }
}

class Result {
    int totalPriorityValue;
    int totalWeight;
    List<CargoItem> itemsChosen;

    public Result(int totalPriorityValue, int totalWeight, List<CargoItem> itemsChosen) {
        this.totalPriorityValue = totalPriorityValue;
        this.totalWeight = totalWeight;
        this.itemsChosen = itemsChosen;
    }
}
