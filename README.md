{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "provenance": [],
      "authorship_tag": "ABX9TyOTwlrx1ERtDSr3rjwKY2g8",
      "include_colab_link": true
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "view-in-github",
        "colab_type": "text"
      },
      "source": [
        "<a href=\"https://colab.research.google.com/github/parmida-pzshk/ISP_OncoDAM/blob/main/README.md\" target=\"_parent\"><img src=\"https://colab.research.google.com/assets/colab-badge.svg\" alt=\"Open In Colab\"/></a>"
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "# **ISP_OncoDAM**\n",
        "This is our project done during the Neuromatch Academy's Impact Scholars Program. The project aimed to investigate the potential of image segmentation to improve the classification model accuracy and generalizability in a cancer diagnostic model trained on two public histopathological image datasets, DigestPath and BACH."
      ],
      "metadata": {
        "id": "9hBZTnvZNRlP"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Google Colab Notebooks**\n",
        "\n",
        "You can run the project directly in Google Colab by clicking the links below:\n",
        "\n",
        "DigestPath dataset:\n",
        "\n",
        "\n",
        "1.   [Unsegmented](https://colab.research.google.com/drive/1P7rVDjEBhcUCjfn2Q8Ex67LBWwbZe5Tr#scrollTo=jqpGPP4brYBI)\n",
        "2.   [Segmented](https://colab.research.google.com/drive/1XOIjudCCx22OBn7RQOKMwWFE1VuNGbpQ#scrollTo=R2mPmzUIjLAr)\n",
        "\n",
        "BACH dataset:\n",
        "\n",
        "\n",
        "1.   [Unsegmented](https://colab.research.google.com/drive/1XOIjudCCx22OBn7RQOKMwWFE1VuNGbpQ#scrollTo=R2mPmzUIjLAr)\n",
        "2.   [Segmented](https://colab.research.google.com/drive/1ly7n19RAaLHcJ_4BIfJHYiLpgseYzihv?usp=sharing#scrollTo=5bRACozxMArY)\n",
        "\n",
        "\n",
        "\n",
        "\n"
      ],
      "metadata": {
        "id": "877XTLgTOBqw"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "!pip install https://github.com/aaren/notedown/tarball/master"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "yy-25ABxSNrx",
        "outputId": "e7445b6a-dbb8-464c-a5ab-e8d602ce4ef0"
      },
      "execution_count": 1,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Collecting https://github.com/aaren/notedown/tarball/master\n",
            "  Downloading https://github.com/aaren/notedown/tarball/master\n",
            "\u001b[2K     \u001b[32m\\\u001b[0m \u001b[32m79.6 kB\u001b[0m \u001b[31m1.0 MB/s\u001b[0m \u001b[33m0:00:00\u001b[0m\n",
            "\u001b[?25h  Preparing metadata (setup.py) ... \u001b[?25l\u001b[?25hdone\n",
            "Requirement already satisfied: nbformat in /usr/local/lib/python3.11/dist-packages (from notedown==1.5.1) (5.10.4)\n",
            "Requirement already satisfied: nbconvert in /usr/local/lib/python3.11/dist-packages (from notedown==1.5.1) (7.16.6)\n",
            "Collecting pandoc-attributes (from notedown==1.5.1)\n",
            "  Downloading pandoc-attributes-0.1.7.tar.gz (2.6 kB)\n",
            "  Preparing metadata (setup.py) ... \u001b[?25l\u001b[?25hdone\n",
            "Requirement already satisfied: six in /usr/local/lib/python3.11/dist-packages (from notedown==1.5.1) (1.17.0)\n",
            "Requirement already satisfied: beautifulsoup4 in /usr/local/lib/python3.11/dist-packages (from nbconvert->notedown==1.5.1) (4.13.3)\n",
            "Requirement already satisfied: bleach!=5.0.0 in /usr/local/lib/python3.11/dist-packages (from bleach[css]!=5.0.0->nbconvert->notedown==1.5.1) (6.2.0)\n",
            "Requirement already satisfied: defusedxml in /usr/local/lib/python3.11/dist-packages (from nbconvert->notedown==1.5.1) (0.7.1)\n",
            "Requirement already satisfied: jinja2>=3.0 in /usr/local/lib/python3.11/dist-packages (from nbconvert->notedown==1.5.1) (3.1.5)\n",
            "Requirement already satisfied: jupyter-core>=4.7 in /usr/local/lib/python3.11/dist-packages (from nbconvert->notedown==1.5.1) (5.7.2)\n",
            "Requirement already satisfied: jupyterlab-pygments in /usr/local/lib/python3.11/dist-packages (from nbconvert->notedown==1.5.1) (0.3.0)\n",
            "Requirement already satisfied: markupsafe>=2.0 in /usr/local/lib/python3.11/dist-packages (from nbconvert->notedown==1.5.1) (3.0.2)\n",
            "Requirement already satisfied: mistune<4,>=2.0.3 in /usr/local/lib/python3.11/dist-packages (from nbconvert->notedown==1.5.1) (3.1.2)\n",
            "Requirement already satisfied: nbclient>=0.5.0 in /usr/local/lib/python3.11/dist-packages (from nbconvert->notedown==1.5.1) (0.10.2)\n",
            "Requirement already satisfied: packaging in /usr/local/lib/python3.11/dist-packages (from nbconvert->notedown==1.5.1) (24.2)\n",
            "Requirement already satisfied: pandocfilters>=1.4.1 in /usr/local/lib/python3.11/dist-packages (from nbconvert->notedown==1.5.1) (1.5.1)\n",
            "Requirement already satisfied: pygments>=2.4.1 in /usr/local/lib/python3.11/dist-packages (from nbconvert->notedown==1.5.1) (2.18.0)\n",
            "Requirement already satisfied: traitlets>=5.1 in /usr/local/lib/python3.11/dist-packages (from nbconvert->notedown==1.5.1) (5.7.1)\n",
            "Requirement already satisfied: fastjsonschema>=2.15 in /usr/local/lib/python3.11/dist-packages (from nbformat->notedown==1.5.1) (2.21.1)\n",
            "Requirement already satisfied: jsonschema>=2.6 in /usr/local/lib/python3.11/dist-packages (from nbformat->notedown==1.5.1) (4.23.0)\n",
            "Requirement already satisfied: webencodings in /usr/local/lib/python3.11/dist-packages (from bleach!=5.0.0->bleach[css]!=5.0.0->nbconvert->notedown==1.5.1) (0.5.1)\n",
            "Requirement already satisfied: tinycss2<1.5,>=1.1.0 in /usr/local/lib/python3.11/dist-packages (from bleach[css]!=5.0.0->nbconvert->notedown==1.5.1) (1.4.0)\n",
            "Requirement already satisfied: attrs>=22.2.0 in /usr/local/lib/python3.11/dist-packages (from jsonschema>=2.6->nbformat->notedown==1.5.1) (25.1.0)\n",
            "Requirement already satisfied: jsonschema-specifications>=2023.03.6 in /usr/local/lib/python3.11/dist-packages (from jsonschema>=2.6->nbformat->notedown==1.5.1) (2024.10.1)\n",
            "Requirement already satisfied: referencing>=0.28.4 in /usr/local/lib/python3.11/dist-packages (from jsonschema>=2.6->nbformat->notedown==1.5.1) (0.36.2)\n",
            "Requirement already satisfied: rpds-py>=0.7.1 in /usr/local/lib/python3.11/dist-packages (from jsonschema>=2.6->nbformat->notedown==1.5.1) (0.23.1)\n",
            "Requirement already satisfied: platformdirs>=2.5 in /usr/local/lib/python3.11/dist-packages (from jupyter-core>=4.7->nbconvert->notedown==1.5.1) (4.3.6)\n",
            "Requirement already satisfied: jupyter-client>=6.1.12 in /usr/local/lib/python3.11/dist-packages (from nbclient>=0.5.0->nbconvert->notedown==1.5.1) (6.1.12)\n",
            "Requirement already satisfied: soupsieve>1.2 in /usr/local/lib/python3.11/dist-packages (from beautifulsoup4->nbconvert->notedown==1.5.1) (2.6)\n",
            "Requirement already satisfied: typing-extensions>=4.0.0 in /usr/local/lib/python3.11/dist-packages (from beautifulsoup4->nbconvert->notedown==1.5.1) (4.12.2)\n",
            "Requirement already satisfied: pyzmq>=13 in /usr/local/lib/python3.11/dist-packages (from jupyter-client>=6.1.12->nbclient>=0.5.0->nbconvert->notedown==1.5.1) (24.0.1)\n",
            "Requirement already satisfied: python-dateutil>=2.1 in /usr/local/lib/python3.11/dist-packages (from jupyter-client>=6.1.12->nbclient>=0.5.0->nbconvert->notedown==1.5.1) (2.8.2)\n",
            "Requirement already satisfied: tornado>=4.1 in /usr/local/lib/python3.11/dist-packages (from jupyter-client>=6.1.12->nbclient>=0.5.0->nbconvert->notedown==1.5.1) (6.4.2)\n",
            "Building wheels for collected packages: notedown, pandoc-attributes\n",
            "  Building wheel for notedown (setup.py) ... \u001b[?25l\u001b[?25hdone\n",
            "  Created wheel for notedown: filename=notedown-1.5.1-py3-none-any.whl size=17499 sha256=4dd1c5d2ee3b59a45bdd2361513dab281716579ffd71596f07a954f8930be97d\n",
            "  Stored in directory: /tmp/pip-ephem-wheel-cache-jl5hj9ew/wheels/25/df/6e/b2da27cafd60355a12e0cef3c83ac00f6630934d4e07afee14\n",
            "  Building wheel for pandoc-attributes (setup.py) ... \u001b[?25l\u001b[?25hdone\n",
            "  Created wheel for pandoc-attributes: filename=pandoc_attributes-0.1.7-py3-none-any.whl size=3007 sha256=271774ae5de24ea50b0e606548264d9c1ce1c05f148d2aaa8fecbf157bc0f5a0\n",
            "  Stored in directory: /root/.cache/pip/wheels/f8/09/7a/f71bd33ccab1f1b74586690933294c642173dffb8f7d74c457\n",
            "Successfully built notedown pandoc-attributes\n",
            "Installing collected packages: pandoc-attributes, notedown\n",
            "Successfully installed notedown-1.5.1 pandoc-attributes-0.1.7\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "!notedown /content/README.ipynb --to markdown --strip > README.md"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "Ec0GYlNgSX6_",
        "outputId": "8ab068f9-1a5c-433f-d111-177013211a49"
      },
      "execution_count": 3,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Traceback (most recent call last):\n",
            "  File \"/usr/local/bin/notedown\", line 8, in <module>\n",
            "    sys.exit(app())\n",
            "             ^^^^^\n",
            "  File \"/usr/local/lib/python3.11/dist-packages/notedown/main.py\", line 312, in app\n",
            "    main(args, help=parser.format_help())\n",
            "  File \"/usr/local/lib/python3.11/dist-packages/notedown/main.py\", line 301, in main\n",
            "    writer.write(notebook, unicode_std_stream('stdout'))\n",
            "  File \"/usr/local/lib/python3.11/dist-packages/nbformat/v4/rwbase.py\", line 132, in write\n",
            "    nbs = self.writes(nb, **kwargs)\n",
            "          ^^^^^^^^^^^^^^^^^^^^^^^^^\n",
            "  File \"/usr/local/lib/python3.11/dist-packages/notedown/notedown.py\", line 434, in writes\n",
            "    body, resources = self.exporter.from_notebook_node(notebook)\n",
            "                      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^\n",
            "  File \"/usr/local/lib/python3.11/dist-packages/nbconvert/exporters/templateexporter.py\", line 429, in from_notebook_node\n",
            "    output = self.template.render(nb=nb_copy, resources=resources)\n",
            "             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^\n",
            "  File \"/usr/local/lib/python3.11/dist-packages/jinja2/environment.py\", line 1295, in render\n",
            "    self.environment.handle_exception()\n",
            "  File \"/usr/local/lib/python3.11/dist-packages/jinja2/environment.py\", line 942, in handle_exception\n",
            "    raise rewrite_traceback_stack(source=source)\n",
            "  File \"/usr/local/lib/python3.11/dist-packages/notedown/templates/markdown.tpl\", line 1, in top-level template code\n",
            "    {% extends 'display_priority.tpl' %}\n",
            "    ^^^^^^^^^^^^^^^^^^^^^^^^^\n",
            "  File \"/usr/local/share/jupyter/nbconvert/templates/compatibility/display_priority.tpl\", line 2, in top-level template code\n",
            "    {%- extends 'display_priority.j2' -%}\n",
            "    ^^^^^^^^^^^^^^^^^^^^^^^^^\n",
            "jinja2.exceptions.TemplateNotFound: display_priority.j2\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "!git clone https://github.com/parmida-pzshk/ISP_OncoDAM\n"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "sSW1cfyYSp0s",
        "outputId": "dfd4b814-430d-4a9a-83c1-7dcb0e3360c3"
      },
      "execution_count": 5,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Cloning into 'ISP_OncoDAM'...\n",
            "remote: Enumerating objects: 10, done.\u001b[K\n",
            "remote: Counting objects: 100% (10/10), done.\u001b[K\n",
            "remote: Compressing objects: 100% (10/10), done.\u001b[K\n",
            "remote: Total 10 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)\u001b[K\n",
            "Receiving objects: 100% (10/10), 876.12 KiB | 5.58 MiB/s, done.\n",
            "Resolving deltas: 100% (2/2), done.\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "!mv /content/README.md /content/ISP_OncoDAM/\n"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "VJuW4SUpS6UF",
        "outputId": "02edbe5a-6227-4581-938d-f8a598e927f0"
      },
      "execution_count": 7,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "mv: cannot stat '/content/README.md': No such file or directory\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "!git config --global user.email \"pezeshkiparmida@gmail.com.com\"\n",
        "!git config --global user.name \"parmida-pzshk\""
      ],
      "metadata": {
        "id": "IgaBme8ITzeM"
      },
      "execution_count": 12,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "%cd /content/ISP_OncoDAM/\n",
        "!git add README.md\n",
        "!git commit -m \"Added README file\"\n",
        "!git push origin main\n"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "TkudBF6ATI1L",
        "outputId": "b1a202ae-12ec-42a7-ce2a-dfef8e9efee6"
      },
      "execution_count": 13,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "/content/ISP_OncoDAM\n",
            "On branch main\n",
            "Your branch is ahead of 'origin/main' by 1 commit.\n",
            "  (use \"git push\" to publish your local commits)\n",
            "\n",
            "nothing to commit, working tree clean\n",
            "fatal: could not read Username for 'https://github.com': No such device or address\n"
          ]
        }
      ]
    }
  ]
}