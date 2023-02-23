# Click and drag belief elicitation interface for qualtrics/limesurvey

![](animated_gif/qualtrics_animated_gif.gif)

This is the Qualtrics/Limesurvey JavaScript plugin for the **Click and Drag** _belief elicitation interface_ presented by [Paolo Crosetto](https://paolocrosetto.wordpress.com/) and [Thomas De Haan](https://sites.google.com/view/thomas-de-haan). Details of the interface, a paper presenting its performance with respect to other interfaces, and oTree code are available on the [Click and Drag website](https://beliefelicitation.github.io/).

## Installation for Limesurvey
### Import the .lsq template file
You can simply download and import this [.lsq template
file](https://raw.githubusercontent.com/beliefelicitation/qualtrics/main/limesurvey_Click-and-drag_elicitation_builder_template.lsq) into your questionnaire.

Click on the image to see the tutorial :

[![Click on the image to see the tutorial](https://img.youtube.com/vi/QEHyH1YbNEQ/0.jpg)](https://www.youtube.com/watch?v=QEHyH1YbNEQ)

## Installation for Qualtrics XM

This plugin will work with all Qualtrics plans that allow you to use JavaScript. This will **not** work with a free account, and also with some paid account. Look into your Qualtrics plan to see if you have access to custom JavaScript for your survey items.

### Import the .qsf template file

To install the plugin, simply download and import the [.qsf template
file](https://raw.githubusercontent.com/beliefelicitation/qualtrics/main/Click-and-drag_elicitation_builder_template.qsf)
into qualtrics :

1.  Download
    [here](https://raw.githubusercontent.com/beliefelicitation/qualtrics/main/Click-and-drag_elicitation_builder_template.qsf)
2.  Navigate to the `Survey` tab and click `Tools`. ![Import Export menu under    Tools](https://www.qualtrics.com/m/assets/support/wp-content/uploads/2021/03/import-export-survey-2.png)
3.  Select `Import/Export`.
4.  Choose `Import survey`.
5.  Click `Choose File` and browse your computer for the `.qsf`
    ![Import Survey window](https://www.qualtrics.com/m/assets/support/wp-content/uploads/2021/03/import-export-survey-4.png)
6.  Select a `Project Category`.
7.  Click `Import`.

### Custom the belief elicitation interface

1.  Click on the question block. ![selecting a question and then clicking   Javascript](https://www.qualtrics.com/m/assets/support/wp-content/uploads/2021/04/JavaScript12.png)
2.  In the `Question behavior` section, select JavaScript.



You will find:

``` javascript
Qualtrics.SurveyEngine.addOnReady(function()
{
   $('.QuestionText').append('<div class="chart" data-n_bins="11" data-min="0" data-step="1" data-y_max="1" data-x_axis_title="Distribution" data-y_axis_title="Probability" data-x_unit="%" data-distribution_result="DistributionResult1" data-distribution_ydata="DistributionYData1" data-distribution_xdata="DistributionXData1" data-distribution_history="DistributionHistory1"></div>');

   $.getScript("https://cdn.jsdelivr.net/gh/beliefelicitation/qualtrics@1.1.4/click-drag-elicitation-qualtrics.js");

});
```

To customize the belief elicitation interface you just need to edit these parameters in the `$('.QuestionText')` line :

``` javascript
'<div class="chart" data-n_bins="11" data-min="0" data-step="1" data-y_max="1" data-x_axis_title="Distribution" data-y_axis_title="Probability" data-x_unit="%" data-distribution_result="DistributionResult1" data-distribution_ydata="DistributionYData1" data-distribution_xdata="DistributionXData1" data-distribution_history="DistributionHistory1"></div>'
```

| Parameter               | Html variable     | Default Value|
|:------------------------|:------------------|:-------------|
| Number of bins          | data-n_bins       | 11           |
| Min value of the x axis | data-min          | 0            |
| Step between bins       | data-step         | 1            |
| Max value of the y axis | data-y_max        | 1            |
| X axis Title            | data-x_axis_title | Distribution |
| Y axis Title            | data-y_axis_title | Probability  |
| X axis unit             | data-x_unit       | \%           |
| DistributionResult             | data-distribution_result      | DistributionResult1         |
| DistributionHistory             | data-distribution_ydata      | DistributionYData1           |
| DistributionXData             | data-distribution_xdata       | DistributionXData1           |
| DistributionYData             | data-distribution_history       | DistributionHistory1           |

Attention, the variable names for the data (DistributionResult1,DistributionYData1,etc.) must be the same as in the "Survey Flow" tab for Embedded data. See : https://www.qualtrics.com/support/survey-platform/survey-module/survey-flow/standard-elements/embedded-data/

The div rendering the chart is injected by jquery (append) in the question block. You can also insert the div in the source code of the question block instead:
``` html
<div class="chart" data-n_bins="11" data-min="0" data-step="1" data-y_max="1" data-x_axis_title="Distribution" data-y_axis_title="Probability" data-x_unit="%" data-distribution_result="DistributionResult1" data-distribution_ydata="DistributionYData1" data-distribution_xdata="DistributionXData1" data-distribution_history="DistributionHistory1"></div>
```

### Libraries js/css (not required for installation)

To find where the js/css scripts are located

1. Navigate to the `Look and Feel` section of your survey, and click on the `Advanced` tab 
2. Edit the `Header`"` section, you will find the libraries scripts and libraries styles

## Output data

The plugin generates the following variables:

| Result               | CSV/Excel column     | Example | Type |
|:------------------------|:------------------|:-------------|:-------------|
|  Coordinates of the bins         | `DistributionResult`| `[[0,0],[0.1,0],[0.2,0],[0.30000000000000004,0.6666666666666666]]`           | List of List |
| History of the coordinates of the bins (time in ms) | `DistributionHistory`          | `[{"delay_ms":0,"xData":[0,0.1,0.2,0.30000000000000004,0.4,0.5,0.6000000000000001,0.7000000000000001,0.8,0.9,1],"yData":[0,0,0,0.6666666666666666,0.3333333333333333,0,0,0,0,0,0]},{"delay_ms":78585,"xData":[0,0.1,0.2,0.30000000000000004,0.4,0.5,0.6000000000000001,0.7000000000000001,0.8,0.9,1],"yData":[0,0,0,0.6666666666666666,0.3333333333333333,0,0,0,0,0,0]}]`            | List of Dict |
| X coordinates of the bins       | `DistributionXData`         |      `0,0.1,0.2,0.30000000000000004`      | List |
| Y coordinates of the bins            | `DistributionYData` | `0,0,0,0.6666666666666666` | List |

In more detail, 

- **DistributionResult** gives you the `[x,y]` position of each bin. The `x` position is the interval of the bin (e.g., if you ask the forecasted inflation for the US in 2024, it is "1%"). The `y` position is the height of that bin, given that the sum of y positions is normalized to 1. The `y` position gives you the subject's subjective probability assigned to that `x` category.

- **DistributionHistory** records the `DistributionResult` after _each_ interaction with the interface (i.e, each click). It can be useful if you want to know the path followed by subjects to get to their final reply. It records the timestamp of the click (`delay_ms`) and the `x` and `y` data at that timestamp. 

- **DistributionXData** is just the `x` dimension of `DistributionResult`

- **DistributionYData** is just the `y` dimension of `DistributionResult`

The labels of the bins and the main question are recorded in the `<div>` component of the qualtrics question. 
