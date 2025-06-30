# ProductRecommender
package com.recommender;
import org.apache.mahout.cf.taste.impl.model.file.FileDataModel;
import org.apache.mahout.cf.taste.model.DataModel;
import org.apache.mahout.cf.taste.impl.similarity.PearsonCorrelationSimilarity;
import org.apache.mahout.cf.taste.similarity.UserSimilarity;
import org.apache.mahout.cf.taste.impl.neighborhood.NearestNUserNeighborhood;
import org.apache.mahout.cf.taste.neighborhood.UserNeighborhood;
import org.apache.mahout.cf.taste.impl.recommender.GenericUserBasedRecommender;
import org.apache.mahout.cf.taste.recommender.Recommender;
import org.apache.mahout.cf.taste.recommender.RecommendedItem;

import java.io.File;
import java.util.List;
import java.util.logging.Level;
import java.util.logging.Logger;

public class ProductRecommender {
    public static void main(String[] args) throws Exception {
        // Optional: disable info logs
        Logger.getLogger("org.apache.mahout").setLevel(Level.SEVERE);

        // Step 1: Load your dataset
        File file = new File("data/dataset.csv");
        DataModel model = new FileDataModel(file);

        // Step 2: Build recommender system
        UserSimilarity similarity = new PearsonCorrelationSimilarity(model);
        UserNeighborhood neighborhood = new NearestNUserNeighborhood(2, similarity, model);
        Recommender recommender = new GenericUserBasedRecommender(model, neighborhood, similarity);

        // Step 3: Produce recommendations for user ID 1
        List<RecommendedItem> recommendations = recommender.recommend(1, 3);

        // Step 4: Print results to console
        if (recommendations.isEmpty()) {
            System.out.println("No recommendations found for user 1.");
        } else {
            System.out.println("Recommendations for user 1:");
            for (RecommendedItem recommendation : recommendations) {
                System.out.println("Item ID: " + recommendation.getItemID() +
                        ", Estimated Rating: " + recommendation.getValue());
            }
        }
    }
}
