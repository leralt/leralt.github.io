## line_image_fitting_op
Line image fitting是一种根据像素强度或颜色来拟合图像中的直线的方法
farthest_point_img_plane_world_range_x是指拟合的直线模型在图像平面上距离线段两端点中离该直线最远的点所对应的世界坐标系中x轴方向的值范围。也就是说，该值表示了线段在x轴上的投影范围，可以用来评估该线段在图像中的倾斜程度。在拟合直线时，该值可以用来限制拟合直线的范围，从而提高直线拟合的准确性。
```cpp
for (auto& laneline : cur_laneline_results_) {
    const auto& all_points = laneline->implane_points;
    std::vector<Point2f> img_selected_points;
    std::vector<Point2f> bv_selected_points;

    // range selection for image/bv fitting points
    auto img_end = std::copy_if(all_points.begin(), all_points.end(), 
        std::back_inserter(img_selected_points), 
        [&img_farthest_point_y_on_image](const auto& pt) {
            return pt.y >= img_farthest_point_y_on_image ||
                (img_selected_points.size() > 0 && img_selected_points.back().y > img_farthest_point_y_on_image && pt.y < img_farthest_point_y_on_image);
        });
    img_selected_points.erase(img_end, img_selected_points.end());

    auto bv_end = std::copy_if(all_points.begin(), all_points.end(), 
        std::back_inserter(bv_selected_points), 
        [&bv_farthest_point_y_on_image](const auto& pt) {
            return pt.y >= bv_farthest_point_y_on_image ||
                (bv_selected_points.size() > 0 && bv_selected_points.back().y > bv_farthest_point_y_on_image && pt.y < bv_farthest_point_y_on_image);
        });
    bv_selected_points.erase(bv_end, bv_selected_points.end());

    // only when points is reduced, then it's necessary to show log
    if (all_points.size() != img_selected_points.size() || all_points.size() != bv_selected_points.size()) {
        AP_LDEBUG(LineImageFittingOP) << "implane points size [all | imag | bv]: " << all_points.size() << " | " << img_selected_points.size() << " | " << bv_selected_points.size();
    }


```

- [x] CondfieldSimplifyPostprocessOP
- [x] LineImageFittingOP
- [x] LineBVFittingOP
- [x] LaneTrackingOP
- [ ] RoadsideBVFittingOP