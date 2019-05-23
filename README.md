# visualise-spacy-tree

An alternative to [SpaCy's](https://spacy.io) [visualizer](https://spacy.io/usage/visualizers#dep), built on [GraphViz](https://graphviz.gitlab.io/). 

![Custom plot image](https://github.com/cyclecycle/visualise-spacy-tree/blob/master/example_plots/custom_plot.png)

## Prerequisites

- [GraphViz](https://graphviz.gitlab.io/). Download and install as per the instructions [here](https://graphviz.gitlab.io/download/).

## Installation

With pip:

```bash
pip install visualise-spacy-tree
```

Or from source:

```bash
git clone https://github.com/cyclecycle/visualise-spacy-tree.git visualise_spacy_tree
cd visualise_spacy_tree
python setup.py install
```

## Usage

```python

# Parse a string to create SpaCy Doc object
import en_core_web_sm

text = 'Forging involves the shaping of metal using localized compressive forces.'

nlp = en_core_web_sm.load()
doc = nlp(text)

# Create the plot
import visualise_spacy_tree
plot = visualise_spacy_tree.plot(doc)

# Write it to a file (it's .png format)
with open('parse_tree.png', 'wb') as f:
    f.write(plot)

# If you're using Jupyter notebook, you can render it inline
from IPython.display import Image, display
display(Image(plot))

# Override node attributes using the underscore extension
from spacy.tokens import Token
Token.set_extension('plot', default={})
for token in doc:
    node_text = '{0} [{1}])'.format(token.orth_, token.i)
    token._.plot['text'] = node_text
    if token.dep_ == 'ROOT':
        token._.plot['color'] = 'green'

'''
See GraphViz docs for reference of possible node attributes:
https://graphviz.gitlab.io/_pages/doc/info/attrs.html
'''

```

## Acknowledgements

Uses:

- [pydot](https://github.com/pydot/pydot)
