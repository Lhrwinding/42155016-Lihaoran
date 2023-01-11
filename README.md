# 42155016-Lihaoran
public class KDTree {
    private static class Node {
        double[] point;
        Node left;
        Node right;

        Node(double[] point) {
            this.point = point;
            left = null;
            right = null;
        }
    }

    private Node root;
    private int k;

    public KDTree(int k) {
        this.k = k;
        root = null;
    }

    public void insert(double[] point) {
        root = insertHelper(root, point, 0);
    }

    private Node insertHelper(Node node, double[] point, int depth) {
        if (node == null) {
            return new Node(point);
        }

        int dim = depth % k;
        if (point[dim] < node.point[dim]) {
            node.left = insertHelper(node.left, point, depth+1);
        } else {
            node.right = insertHelper(node.right, point, depth+1);
        }

        return node;
    }

    public List<double[]> range(double[] minCoords, double[] maxCoords) {
        List<double[]> result = new ArrayList<>();
        rangeHelper(root, minCoords, maxCoords, result, 0);
        return result;
    }

    private void rangeHelper(Node node, double[] minCoords, double[] maxCoords, List<double[]> result, int depth) {
        if (node == null) {
            return;
        }

        int dim = depth % k;
        if (minCoords[dim] <= node.point[dim] && node.point[dim] <= maxCoords[dim]) {
            result.add(node.point);
        }

        if (minCoords[dim] <= node.point[dim]) {
            rangeHelper(node.left, minCoords, maxCoords, result, depth+1);
        }

        if (node.point[dim] <= maxCoords[dim]) {
            rangeHelper(node.right, minCoords, maxCoords, result, depth+1);
        }
    }
}
